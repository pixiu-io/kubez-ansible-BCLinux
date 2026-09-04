# Kubez-ansible-BCLinux

`kubez-ansible-BCLinux` 是面向 **BigCloud Enterprise Linux（bclinux）** 定制的 [kubez-ansible](https://github.com/pixiu-io/kubez-ansible) 分支项目，用于在 BCLinux 上快速部署 Kubernetes 集群及云原生应用。

![Build Status][build-url]
[![License][license-image]][license-url]

## 项目介绍

面向 **BigCloud Enterprise Linux（bclinux）** 定制。

**架构支持：**

| 部署方式 | 支持架构 |
|----------|----------|
| 在线     | arm64（aarch64）、amd64（x86_64） |
| 离线     | 仅 arm64（aarch64） |

```text
NAME="BigCloud Enterprise Linux"   # 部分版本也可能为 BigCloud
ID="bclinux"
VERSION_ID="21.10"
# 在线：arm64 / amd64；离线：仅 arm64（镜像标签 arm64-bclinux / amd64-bclinux）
```

其它发行版请使用上游项目：[pixiu-io/kubez-ansible](https://github.com/pixiu-io/kubez-ansible)。

## 常见问题

- [kube-proxy / Calico 起不来：需切换 nftables](docs/faq.md#kube-proxy--calico-起不来需切换-nftables)

## 学习分享

- [go-learning](https://github.com/caoyingjunz/go-learning)

## 沟通交流

- 搜索微信号 `yingjuncz`, 备注（github）, 验证通过会加入群聊
- [bilibili](https://space.bilibili.com/3493104248162809?spm_id_from=333.1007.0.0) 技术分享

Copyright 2019 caoyingjun (cao.yingjunz@gmail.com) Apache License 2.0

[build-url]: https://github.com/gopixiu-io/kubez-ansible/actions/workflows/ci.yml/badge.svg
[license-image]: https://img.shields.io/badge/license-Apache%202-4EB1BA.svg
[license-url]: https://www.apache.org/licenses/LICENSE-2.0.html
