---
title: Deploy Config
---

我们将在本节中介绍 Nocalhost 部署配置。

Nocalhost 支持 Helm，Rawmanifest 和 Kustomiz 的 K8S 应用程序的部署。
当应用工作负载，部署环境注入，生命周期钩等时，它们可以提供诸如依赖订单规范之类的功能。
此外，它们还支持多种配置方法，例如配置，注释等。

!!! info "在开始本节之前，请确保您已经知道如何配置 Nocalhost。"

    如果没有，请首先阅读 [Nocalhost概述](../config-overview-en.md)。

## [快速启动 - 基本的部署配置](quickstart.md)

我们介绍了`Deploy Config`提供的一些功能，因此在本节中，我们将提供一些示例，以更多地说明有关基本 nocalhost`Deploy Config`和安装的更多信息。

## [Nocalhost 部署配置规范](spec.md)

知道最基本的 nocalhost`Deploy Config`,我们将在本节中介绍特定的部署配置, 包括依赖订单规范在启动工作量时，部署 env 注入，挂钩等.

## [Dep 组件和其他方法](advance.md)

Nocalhost `Deploy Config`支持多种方法，例如 ConfigMap，注释等。
实际上，这些方法也适用于`Deploy Config`,但是有些功能需要与 K8S Webhook 结合使用, Nocalhost 中的`Nocalhost-Dep`组件发挥了作用.
`Nocahost Server` 将自动部署此组件, 因此，如果您不使用`Nocalhost Server`, 您需要部署额外的组件`Nocalhost Dep`.

## [Config.yaml 句法](syntax.md)

为了提高 Nocalhost 配置的可重复性和灵活性并避免重复配置，Nocahost 提供了环境可变注入，YAML 包括语法。
