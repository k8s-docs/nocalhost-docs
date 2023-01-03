---
title: What is Nocalhost Config?
---

我们将介绍 Nocalhost 的配置，如何修改它及其功能等。
如果您没有 Nocalhost 配置的概念，或者想了解更多有关其结构和功能的概念，请阅读本节。

## Nocalhost 配置

开发配置是围绕`DevMode`设置的, 例如，应使用哪个镜像输入`DevMode`, 开发容器中是否需要持久性, 哪些文件应同步到开发容器, 如何通过一键在容器中进行调试和运行服务, 等等
Nocalhost 中的`DevMode`将更容易使用正确且适当的开发配置使用。

总体而言，开发配置是更好地定义`DevMode`的行为。

!!! tip "开发配置和部署配置"

    Nocalhost的配置可以分为两个部分：开发配置和部署配置。

    - 部署配置定义了K8S应用程序如何部署，包括依赖项控制，可变注入等。
    - 开发配置是围绕`DevMode`设置的, 例如，在`DevMode`中应使用哪个镜像, 开发容器中是否需要持久性, 哪些文件应同步到开发容器, 如何通过一键在容器中进行调试和运行服务, 等等

通常，只有 **开发配置** 需要关注。
**在配置文档中，我们提到的配置是指开发配置，除非另有说明。**

## 查看并保存配置

### 查看配置

如果您从未为任何工作负载配置 Nocalhost, ++rbutton+"Dev Config"++ , 然后，您将看到如下的空配置, 这些都是在 nocalhost 的`DevMode`中使用的.

!!! tip "配置不是必须的"

    您可以在没有任何配置的情况下输入`DevMode`。

```yaml
name: coredns
serviceType: deployment
containers:
  - name: ""
    dev:
      gitUrl: ""
      image: ""
      shell: ""
      workDir: /home/nocalhost-dev
      storageClass: ""
      resources: null
      persistentVolumeDirs: []
      command: null
      debug: null
      sync: null
      env: []
      portForward: []
```

### 更新配置

我们可以进行任何更改，并在 IDE 插件侧使用 ++cmd+s++ 或 ++ctrl+s++ 来保存它们。

### 配置结构

配置顶层的`name`和`serviceType`表明该配置属于`coredns`的`deployment`。
配置的内容在`containers`中，这是一个数组，可以为一个工作负载中的多个容器设置不同的配置。

## 正确配置容器

### 声明容器名称

首先，您必须声明`containers.[].name`中每个容器的名称，以区分每个容器。

例如，如果工作负载中有两个容器，`ContainerA`和`ContainerB`(注意，这只是一个示例，容器应该根据您的实际工作负载命名)，那么您需要如下声明名称:

```yaml
name: coredns
serviceType: deployment
containers:

  - name: "ContainerA"
    dev:
        image: "!!! examplelatest"
        ..........

  - name: "ContainerB"
    dev:
        image: "foo:bar"
        ..........
```

当然，如果你只需要开发`ContainerB`，你可以只配置它。如下:

```yaml
name: coredns
serviceType: deployment
containers:

  - name: "ContainerB"
    dev:
        image: ""
        ..........
```

### 如何配置每个配置项目？

这一部分确实需要很长很详细的解释，但这里我们首先给出一个简单的示例，以便快速开始 Nocalhost 配置。

!!! info "需要一个应用程序来操作?"

    如果没有工作负载需要操作，可以使用以下命令安装演示应用程序:

    ```shell
    kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.10/samples/bookinfo/platform/kube/bookinfo.yaml
    ```

右键单击从未配置过的工作负载，例如 details-v1。
点击`DevConfig`，然后你会看到一个空模板。
让我们做一些更改，例如向开发容器添加一个额外的 env(开发容器将从原始容器继承环境变量):

```yaml
name: details-v1
serviceType: deployment
containers:
  - name: details
    dev:
      env:
        - name: nocalhost
          value: example
```

使用 ++cmd+s++ 或 ++ctrl+s++ 保存更改，然后进入`DevMode`。
由于它只是配置过程的演示，您可以在这里选择任何本地目录，其内容将同步到开发容器，然后您可以选择任何开发映像。

启动`DevMode`后，在终端输入`env | grep nocalhost`。
可以看到，环境变量已经正确地注入了。

```shell
>  env | grep nocalhost
PWD=/home/nocalhost-dev
OLDPWD=/home/nocalhost-dev
nocalhost=example
```

!!! tip "更多的信息"

    要了解更多配置项和功能，请参见[Nocalhost提供哪些配置?](config-spec.md)

## 开发配置的功能

此外，Nocalhost 配置的设计还带来了一些功能。了解这些功能可能会帮助您更好地使用 Nocalhost。

### 生效

nocalhost 的开发配置不会立即生效。
它需要保存，然后 **重新进入`DevMode`** 以使其工作。

### 生命周期

Nocalhost 将在每个命名空间中创建一个`secret`作为`迷你数据库`，前缀为`dev.nocalhost.application.`。
配置将保存在这个`secret`中。

数据将被保存在这个`secret`中，直到它被销毁。

!!! info "HELM 申请"

    如果它是一个HELM应用程序，例如，它的`Release.Name`是`bookinfo`，这个`secret`将被命名为`dev.nocalhost.application.bookinfo`。
    此外，在卸载`bookinfo`后，存储在这个`secret`中的数据将被销毁。

### 能见度

从存储设计可以发现，Nocalhost 的配置是共享的。
具体来说，在同一个集群的同一个`名称空间`中，一个工作负载的配置在任何设备上都是可见的，并且可以修改(具有[RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)的`secret`的修改权限)。
所有设备获得的配置都来自相同的副本。
