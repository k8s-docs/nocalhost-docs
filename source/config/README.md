# 概述

## [配置概述](config-overview.md)

它是什么，如何配置以及它具有什么功能？

本节介绍了一些信息，例如 Nocalhost 配置是什么，如何修改配置以及具有什么功能。
**如果您没有 Nocalhost 配置的概念** 或想了解 Nocalhost 配置的结构和功能，则可以读取本节。

## [提供什么配置？](config-spec.md)

开发配置定义了输入`DevMode`的行为。
如果 **您想在输入`DevMode`时进行一些自定义配置**，则开发配置将很有帮助。
如果您想知道 Nocalhost 提供的配置，则可以阅读本节。

## [支持哪种配置方式？](config-spec.md)

Nocalhost 支持多种配置 Devmode 的方法，并为各种丰富的用法方案提供支持。
最常见的配置方法是右键单击 IDE 插件中的特定工作负载，然后选择`DevConfig`以输入开发配置编辑 UI。

此外，Nocalhost 还支持将开发配置放置在 **源目录，ConfigMap 和注释** 中。
例如，可以通过上述配置方式在过程中或在 Helm 图中的 CD 上进行配置，以避免重复配置或自定义配置，等等。

如果您想了解有关多种配置开发配置方式的更多信息，则可以单击本节的详细信息。

## [部署配置](deployment/README.md)

Nocalhost 具有应用程序部署的功能。在`Nocalhost Server`下，这是一个高频功能。
Nocalhost 为工作负载提供了 **依赖控制和 ENV 注入** 之类的功能。

!!! tip "Tips"

    如果您不使用`Nocalhost Server`或不需要诸如Workload依赖控制和ENV注入或具有自己的完整部署方式之类的功能，则无需阅读本节。

Nocalhost 支持 Helm，RawManifest 和 Kustomize 用于部署 K8S 应用程序，它还支持多种配置方式，例如 **ConfigMap 和注释**。
