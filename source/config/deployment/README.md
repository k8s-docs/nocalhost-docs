---
title: Deploy Config
---

[Overview](config-en.md) / [Deploy Config](config-deployment-en.md)

We will introduce Nocalhost Deploy config in this section.

Nocalhost supports the deployment of K8s applications by Helm, RawManifest and Kustomiz. They can provide functions such as dependency order specification when apply the workload, deployment env injection, life cycle hook, etc. Moreover, they also support multiple configuration methods, such as Configmap, Annotation, etc.

!!! info "PRE-REQUIRE"

    在开始本节之前，请确保您已经知道如何配置Nocalhost。如果没有，请首先阅读[Nocalhost概述]config-overview-en.md）。

## [快速启动 - 基本的 Nocalhost 部署配置](config-deployment-quickstart.md)

We have introduced some functions provided by `Deploy Config`, so in this section, we will give a few examples to explain more about the basic Nocalhost `Deploy Config` and installation.

## [Nocalhost 部署配置规范](config-deployment-spec.md)

After knowing the most basic Nocalhost `Deploy Config`, we will introduce the specific deployment configurations in this section, including dependency order specification when initiating the workload, deployment env injection, hook, etc.

## [Dep 组件和其他方法](config-deployment-advance.md)

Nocalhost `Dev Config` supports multiple methods, such as ConfigMap, Annotations, etc. In fact, these methods are also applicable in `Deploy Config`, but some functions need to work in conjunction with K8s WebHook, and the `Nocalhost-Dep` component in Nocalhost plays that role. `Nocahost Server` will automatically deploy this component, so if you do not use `Nocalhost Server`, you need to deployment extra component `Nocalhost Dep`.

## [Config.yaml 句法](config-deployment-syntax.md)

To improve the reusability and flexibility of Nocalhost configuration and avoid repeated configure, Nocahost provides environment variable injection and yaml include syntax.
