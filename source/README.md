---
title: 什么是 Nocalhost?
---

:link: https://nocalhost.dev

Nocalhost 是一个开源 IDE 插件，用于云原生应用程序开发:

- **直接在 K8s 内部构建、测试和调试应用程序**
- **IDE 支持:** 即使在远程 Kubernetes 群集中，也可以在 IDE 中使用相同的调试和开发经验
- **使用即时文件同步开发:** 立即将代码更改为远程容器，而无需重建镜像或重新启动容器。

![](./img/intro/coding-in-cluster.gif)

## 它是如何工作的？

Nocalhost 由单个二元 CLI 和 IDE 扩展组成。理想情况下，您将其直接与您喜欢的 IDE 一起使用。
Nocalhost 不需要服务器端组件，因为它像 Kubectl 一样，直接使用 Kubeconfig 通信到 Kubernetes 群集。

![](./img/intro/how-it-works.webp)

## 为什么要 Nocalhost？

构建 Kubernetes 应用程序通常很困难，对于大型开发人员来说，这甚至更难。Nocalhost 提供了构建云本地应用程序的最有效的方法。

使用 Nocalhost 直接在 Kubernetes 内部开发的优点是：

- **生产环境相似性** - 开发环境与您的生产环境非常相似，这让您更有信心，当新特性发布时，一切都将在生产环境中正常工作。
- **加快反馈** - 通过文件同步，您的代码更改可以立即在容器中生效，而无需重新构建映像或重新部署容器。
- **灵活缩性** - 开发人员不需要担心本地资源不足。
- **降低成本** - 更有效地使用资源并降低 IT 设施成本

在 Kubernetes 群集中开发可能在以下情况下很有用:

- 本地资源的局限
- 想在类似生产的环境中测试您的应用程序
- 想调试很难在本地机器上复制的问题
- 应用程序需要访问集群内部服务（例如群集 DNS）

## 主要特征

### 在 K8s 中进行编码

Nocalhost 是预配置的，可以与您最喜欢的 IDE 一起工作，您可以一键连接到任何 Kubernetes 集群，并享受在集群内编码，摆脱讨厌的本地环境配置。

### 即时文件同步

每当您进行更改时，Nocalhost 可以自动将代码同步到容器。通过这种方式，消除了提交、构建和推送的循环，大大加快了开发的反馈循环。所以你可以在一秒钟内看到更新。

![](./img/intro/dev-circle.jpg)

### 为协作而建

Nocalhost 帮助您的团队标准化开发工作流程，而不需要团队中的每个人都成为 Kubernetes 专家。

- **Kubernetes 和 DevOps 专家** 在您的团队中可以通过 Nocalhost Server 配置和管理集群，应用程序，DevSpace 和用户，阅读更多关于[Nocalhost Server](./server/server-overview)
- **团队中的开发人员** 可以轻松地检出项目，并在 Kubernetes 集群中开始编码和调试，而无需成为 Kubernetes 专家。

### 兼容性

Nocalhost 进行了许多 Kubernetes 发行的战斗测试:

- **本地 Kubernetes 集群** 比如 minikube, Microk8s, K3s 和 Docker Desktop
- **托管 Kubernetes 群集** 比如 TKE (Tencent), ACK (Alibaba Cloud), GKE (Google), Microsoft Azure
- **自管 Kubernetes 群集** (例如，用 KubeSphere 或 Rancher 创建)
