# 常见问题

## kube-proxy / Calico 起不来：需切换 nftables

部分 BCLinux（如 21.10）搭配较新内核时，可能只提供原生 nftables 的 `nft_masq`，**没有** iptables 兼容层所需的 `xt_MASQUERADE`。此时若仍使用默认的 **iptables** 模式，会出现：

- `kube-proxy` 日志反复报错：`Extension MASQUERADE revision 0 not supported, missing kernel module?` / `iptables-restore ... KUBE-POSTROUTING`
- ClusterIP（如 `10.254.0.1:443`）不通
- `calico-node` 的 `install-cni` / `calico-typha` 访问 API 超时
- 节点 `NotReady`（`cni plugin not initialized`）
- Felix 同样用 iptables 写 NAT 失败，`calico-node` 长期 `0/1 Ready`（`felix is not ready`）

可先在节点上确认：

```bash
lsmod | grep -E 'nf_nat|xt_MASQUERADE|nft_masq|iptable_nat'
modprobe xt_MASQUERADE; echo exit=$?
```

若已有 `nft_masq`，但 `xt_MASQUERADE` 不存在，建议按下面方式切到 **nftables**。

### 1. kube-proxy 改为 nftables

```bash
kubectl -n kube-system edit cm kube-proxy
# 将 KubeProxyConfiguration 中的 mode 设为：
#   mode: "nftables"

kubectl -n kube-system delete pod -l k8s-app=kube-proxy
kubectl -n kube-system logs -l k8s-app=kube-proxy --tail=50
```

成功标志：日志出现 `Using nftables Proxier`，且不再刷 `iptables-restore` / `MASQUERADE` 错误。首次启动时可能有一次 `list table ip kube-proxy: No such file or directory`，属于表尚未创建，可忽略。

验证 ClusterIP：

```bash
curl -vk --connect-timeout 3 https://10.254.0.1:443/healthz
```

### 2. Calico Felix 改为 nftables

kube-proxy 修好后，若 `calico-node` 仍 Ready 失败且日志含同样的 MASQUERADE / `iptables-nft-restore` 错误，需开启 Felix 的 nftables：

```bash
kubectl patch felixconfiguration default --type merge -p '{"spec":{"nftablesMode":"Enabled"}}'
kubectl -n kube-system delete pod -l k8s-app=calico-node
kubectl get pod -n kube-system -l k8s-app=calico-node -w
```

成功标志：`calico-node` 变为 `1/1`，日志不再反复 `Failed to program iptables`。

### 3. 说明

- 单节点上 readiness 中出现 `BGP peering established = 0` 通常可忽略；优先排除 Felix 的 MASQUERADE 失败。
- 更彻底的做法是换带完整 netfilter（含 `xt_MASQUERADE`）的内核；nftables 模式是当前内核能力下的实用方案。
- 部署配置里 `kube_proxy_mode` 仍以 `iptables` / `ipvs` 为主（见 `etc/kubez/globals.yml`）。在 BCLinux 上若踩到上述问题，按本节在集群内手动改为 `nftables`。
