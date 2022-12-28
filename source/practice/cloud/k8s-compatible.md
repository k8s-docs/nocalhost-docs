---
title: Kubernetes Compatibility
id: k8s-compatible
---

# Kubernetes 兼容

!!! caution

    现在，我们一直在使用不同的Kubernetes群集进行战斗，测试结果正在更新。如果您在使用Nocalhost时遇到问题，请随时与我们联系。

## 本地的 Kubernetes 群集

| Product                                                          | Version          | Kubernetes Version | Testing Result                               |
| ---------------------------------------------------------------- | ---------------- | ------------------ | -------------------------------------------- |
| [Minikube](https://github.com/kubernetes/minikube)               | 1.22             | 1.21.2             | <strong className="pass-tag">passed</strong> |
| [Docker Desktop](https://www.docker.com/products/docker-desktop) | 3.5.2 (3.5.2.18) | 1.21.2             | <strong className="pass-tag">passed</strong> |
| [K3s](https://k3s.io/)                                           |                  | 1.21.2             |                                              |
| [MicroK8s](https://microk8s.io/)                                 |                  | 1.21.2             |                                              |
| [K3d](https://github.com/rancher/k3d)                            | 4.4.7            | 1.21.2             | <strong className="pass-tag">passed</strong> |

## 托管 Kubernetes 群集（公共云）

| Company                                                               | Kubernetes Version | Testing Result | Tutorial |
| --------------------------------------------------------------------- | ------------------ | -------------- | -------- |
| Tencent TKE                                                           |                    |                |          |
| Alibaba Cloud                                                         |                    |                |          |
| [Huawei CCE](https://www.huaweicloud.com/intl/en-us/product/cce.html) |                    |                |          |
| Qing Cloud                                                            |                    |                |          |
| Rancher RKE                                                           |                    |                |          |
| Rancher K3s                                                           |                    |                |          |
| Google GKE                                                            |                    |                |          |
| Microsoft Azure                                                       |                    |                |          |
| Amazon EKS                                                            |                    |                |          |

## Kubernetes 平台

| Product    | Version | Kubernetes Version | Testing Result |
| ---------- | ------- | ------------------ | -------------- |
| KubeSphere |         |                    |                |
