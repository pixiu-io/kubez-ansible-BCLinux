# Kubez-ansible-BCLinux

`kubez-ansible-BCLinux` 是面向 **BigCloud Enterprise Linux（bclinux）** 定制的 [kubez-ansible](https://github.com/pixiu-io/kubez-ansible) 分支项目，用于在 BCLinux 上快速部署 Kubernetes 集群及云原生应用。

![Build Status][build-url]
[![License][license-image]][license-url]

## 项目介绍

- **定位**：仅支持 BCLinux，移除上游对 Ubuntu / Debian / Rocky / openEuler / Kylin / CentOS 等系统的适配分支。
- **包管理**：统一使用 **yum** 安装依赖、运行时与 Kubernetes 组件。
- **识别方式**：Ansible 以 `ansible_distribution == "BigCloud Enterprise Linux"` 为准（对应 `/etc/os-release` 中 `NAME="BigCloud Enterprise Linux"`，`ID=bclinux`）。
- **适用版本**：已在 BigCloud Enterprise Linux 21.10（LTS，Euler 系）场景验证为目标系统。

```text
NAME="BigCloud Enterprise Linux"
ID="bclinux"
VERSION_ID="21.10"
```

其它发行版请使用上游项目：[pixiu-io/kubez-ansible](https://github.com/pixiu-io/kubez-ansible)。

## Getting Started

阅读在线文档或视频了解用法：[kubez-ansible 介绍](https://www.bilibili.com/video/BV1L84y1h7LE/)。

部署节点执行依赖安装：

```shell
curl https://raw.githubusercontent.com/pixiu-io/kubez-ansible-BCLinux/master/tools/setup_env.sh | bash
```

## 学习分享

- [go-learning](https://github.com/caoyingjunz/go-learning)

## 沟通交流

- 搜索微信号 `yingjuncz`, 备注（github）, 验证通过会加入群聊
- [bilibili](https://space.bilibili.com/3493104248162809?spm_id_from=333.1007.0.0) 技术分享

Copyright 2019 caoyingjun (cao.yingjunz@gmail.com) Apache License 2.0

[build-url]: https://github.com/gopixiu-io/kubez-ansible/actions/workflows/ci.yml/badge.svg
[license-image]: https://img.shields.io/badge/license-Apache%202-4EB1BA.svg
[license-url]: https://www.apache.org/licenses/LICENSE-2.0.html
