---
title: 开发容器配置
---

## 文件同步的远程目录

!!! example

    ```yaml
    name: nocalhost-api
    serviceType: deployment
    containers:
      - name: nocalhost-api
        dev:
          workDir: /home/nocalhost-dev
    ```

进入 DevMode 后，用户需要在 IDE 插件中选择一个本地目录，或者右键单击目标工作负载并选择`Associate Local DIR`。
这个本地选择的目录将在 DevMode 下与容器的`workDir`同步。

`workDir` 默认是 `/home/nocalhost-dev`

!!! danger "关于 workDir 的说明"

    `workDir`使用emptyDir在`container`中共享，所以这个目录在开始时是空的。

## DevImage

!!! example

    ```yaml
    name: nocalhost-api
    serviceType: deployment
    containers:
      - name: nocalhost-api
        dev:
          image: codingcorp-docker.pkg.coding.net/nocalhost/dev-images/golang:zsh
    ```

DevImage 是 DevMode 的基础，它可以被看作是一个'remote Linux'。
如果你想正确编译和运行从本地同步的文件，你必须使用正确的 DevImage。
Nocalhost 提供多个正式的 DevImage，如果在进入 DevMode 前未配置该字段，则需要选择或输入一个 DevImage 才能继续。

官方的 DevImages 是常规的镜像，没有任何特殊的变化。
除了 Java 中的 JRE、Maven 等各种语言的基本环境，还有一些内置的基础软件，如 git、openssh-client、zsh、bash、net-tools、tmux 等。
如果没有适合您的官方映像，您可以自定义自己的 DevImage。
DockerFile 在[dev-container](https://github.com/nocalhost/dev-container)中.

!!! tip "制作你的 DevImage"

    如果您想自定义DevImage，请将其放置在可以由您的K8s集群提取的存储库中。

## 遥控容器中的 Shell

!!! example

    ```yaml
    name: nocalhost-api
    serviceType: deployment
    containers:
      - name: nocalhost-api
        dev:
          shell: /bin/zsh
    ```

shell 不是必须配置的，默认为`zsh || bash || sh`。
使用合适的 Shell 通常会使事情变得更简单、更高效，例如 zsh 提供的自动补充和历史补充功能。

当然，shell 配置也依赖于 DevImage。
如果您的 DevImage 不支持 zsh，即使您将 zsh 配置为 shell，它也将无法工作。
它会依次查找 zsh、bash 和 sh，直到找到一个可用的。

## 外存挂载

!!! example

    ```yaml
    name: nocalhost-api
    serviceType: deployment
    containers:
      - name: nocalhost-api
        dev:
          persistentVolumeDirs:
            - path: /the/path/you/want/to/persistent/in/container
              capacity: 10Gi
            - path: /other
              capacity: 10Gi
          storageClass: cbs
    ```

我们知道，如果目录没有保存在 K8s 容器中，那么在容器关闭或重新启动后，先前生成的数据将丢失，例如同步文件、编译的内容、构造的内容等。
在 Dev Container 中启用持久性可以大大减少这种损失。

持久性包括两部分:

- 哪些目录需要持久化。
  允许不配置该参数，默认值为空。
  `path`表示需要在 DevImage 中持久化的目录，`capacity`表示为该目录持久化分配的空间。
  `persistentVolumeDirs`是一个用于配置多路径/容量的数组。
- `storageClass`。
  持久化需要`storageClass` (`kubectl get storageClass`)。
  如果不配置`storageClass`， Nocalhost 将使用集群中默认的`storageClass`创建 PVC。
  如果你配置了`storageClass`， PVC 将由相应的`storageClass`创建。

!!! info "请注意"

    `capacity`需要满足k8的资源限制。

## 资源请求和约束

!!! example

    ```yaml
    name: nocalhost-api
    serviceType: deployment
    containers:
      - name: nocalhost-api
        dev:
          resources:
            limits:
              memory: 4Gi
              cpu: "1"
            requests:
              memory: 2Gi
              cpu: "0.5"
    ```

Nocalhost Dev Container 继承了原始容器的资源设置。
如果原始容器中没有配置，命名空间中也没有`limitranges` (`kubectl get limitranges`)， Dev container 将没有资源约束。

一般情况下，进入 DevMode 后，使用的资源量将超过原始映像。
因此，原有容器的资源配置往往会导致 DevMode 失败，例如内存不足导致 OOM。
当这种情况发生时，你需要配置`resources`为 DevImage 提供更多的资源。

!!! info "请注意"

    `memory` and `cpu`需要满足k8s的资源限制。

## SideCar 镜像自定义

!!! example

    ```yaml
    name: nocalhost-api
    serviceType: deployment
    containers:
      - name: nocalhost-api
        dev:
          sidecarImage: nocalhost-docker.pkg.coding.net/nocalhost/public/nocalhost-sidecar:sshversion
    ```

`sidecarImage`是进入 DevMode 的必要映像，用于代码同步、调试连接管理等。
`sidecarImage`默认为`docker.pkg.coding.net/nocalhost/public/nocalhost-sidecar:sshversion`，不需要手动配置。

如果您的集群由于网络原因无法获得该映像，您可以提取该映像并将其推到集群可以访问的映像存储库，然后将其配置为一个新地址。

## Patches(补丁)

!!! example

    ```yaml
    name: nocalhost-api
    serviceType: deployment
    containers:
      - name: nocalhost-api
        dev:
          patches:
            - patch: '[{"op": "add","path":"/metadata/annotations/nocalhost-patch","value":"hello-world"}]'
              type: json
            - patch: '{"spec":{"template":{"spec":{"containers":[{"name":"nocalhost-dev","imagePullPolicy":"IfNotPresent","resources":{"limits":{"cpu":"2"}}}]}}}}'
            - patch: '{"spec":{"template":{"spec":{"containers":[{"name":"nocalhost-sidecar","resources":{"limits":{"cpu":"2"}}}]}}}}'
              type: strategic
    ```

`patches`提供类似于`kubectl patch`的功能。
用户可以使用`patches`灵活地修改 Nocalhost DevMode 中工作负载的 Spec。

在其中：

- **type**: 补丁类型。可选值为`json`, `merge`, `strategic`默认值为`strategic`。
- **patch**: 补丁内容

为了便于理解，`type` and `patch` 可以分别理解为`kubectl patch` 命令的`--type` and `--patch`参数。
要了解更多关于`kuebctl patch`的信息，请参见[更新 K8s API 对象通过 kubectl 补丁](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/update-api-object-kubectl-patch/)

!!! caution

    Nocalhost不会验证补丁的内容，可能会因为补丁的内容不正确导致Nocalhost无法进入DevMode。请确保补丁是正确的。
