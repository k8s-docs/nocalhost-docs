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

如果您从未为任何工作负载配置 Nocalhost, **右键单击并选择 `Dev Config`**, 然后，您将看到如下的空配置, 这些都是在 nocalhost 的`DevMode`中使用的.

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

我们可以进行任何更改，并在 IDE 插件侧使用`Comm`+`S` or `Ctrl`+`S`来保存它们。

## Nocalhost 配置的结构

The `name` and `serviceType` at the top level of the configuration indicate that this configuration belongs to the `deployment` of `coredns` . The content of the configuration is in `containers` , which is an array and can set different configurations for multiple containers in one workload.

## 正确配置容器

### 声明容器名称

首先，您必须声明`containers.[].name`中每个容器的名称，以区分每个容器。

For example, if there are two containers in the workload, `ContainerA` and `ContainerB` (note that this is just an example, and the container should be named according to your real workload) , then you need to declare the names as follows:

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

Surely, if you only need to develop `ContainerB` , you can configure it only. As follows:

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

This part indeed requires a long and detailed explanation, but we first give a simple example here to offer a quick start for Nocalhost configuration.

!!! info "Need an application to operate?"

    If there is no workload to operate, you can use the following command to install a demo application:

    ```shell
    kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.10/samples/bookinfo/platform/kube/bookinfo.yaml
    ```

Right-click a workload that has never been configured, such as details-v1. Click `DevConfig`, then you will see an empty template. Let's make some changes, such as adding an additional env to the development container (the development container will inherit the environment variables from the original container):

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

Using `Comm`+`S` or `Ctrl`+`S` to save the changes, and then enter the `DevMode`. Since it is just a demonstration of the configuration process, you can select any local directory here and its content will be synchronized to the development container, and then you can select any development image.

Type `env | grep nocalhost ` in the terminal after initiating the `DevMode`. As you can see, the environment variables have been injected correctly.

```shell
>  env | grep nocalhost
PWD=/home/nocalhost-dev
OLDPWD=/home/nocalhost-dev
nocalhost=example
```

!!! tip "More information"

    To learn more about configuration items and corresponding functions, see [What configurations does Nocalhost offer?](config-spec-en.md)

## 开发配置的功能

此外，Nocalhost 配置的设计还带来了一些功能。了解这些功能可能会帮助您更好地使用 Nocalhost。

### 生效

The development configuration of nocalhost does not take effect immediately. It needs to be saved and then re-enter the `DevMode` to make it work.

### 生命周期

Nocalhost will create a `secret` in each namespace as a "mini database", prefixed with `dev.nocalhost.application.`. The configuration will be saved in this `secret` .

Data will be remained in this `secret` until it is destroyed.

!!! info "HELM application"

    If it is a HELM application, for example, its `Release.Name` is `bookinfo`, this `secret` will be named `dev.nocalhost.application.bookinfo`. Moreover, the data stored in this `secret` will be destroyed after uninstalling `bookinfo` .

### 能见度

From the storage design, you can find that the configuration of Nocalhost is shared. Specifically, in the same `namespace` of the same cluster, the configuration of one workload is visible on any device and can be modified (with the modification permission of the `secret` of [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) ). The configuration all devices get is from the same duplicate.
