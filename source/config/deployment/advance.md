---
title: 高级配置
---

Nocalhost 的开发配置支持多种开发方式，例如 ConfigMap，注释等。

实际上，这些配置方法**也适合**部署配置。
但是，需要通过 K8S Admission Webhook 来实现某些功能。
在 Nocalhost 中，一个称为`Nocalhost-Dep`的组件扮演此角色，`Nocalhost Server`将自动为您部署此组件。

if you do not use `Nocalhost Server`, then additional deployment of `Nocalhost Dep` is required.

!!! info

    [Nocalhost提供哪些部署配置？](./spec.md) 介绍哪些部署配置需要额外部署`nocalhost dep`.

## 使用 ConfigMap 进行部署配置

To make a simple deployment configuration is mentioned in [Introduction to Deployment Configuration Nocalhost Basic Deployment Configuration](./quickstart.md). Combining with [What configuration methods are supported by Nocalhost-place the configuration in configmap](../configure.md#configuration-in-configmap), we can get the configuration:

We have prepared an demo for this, which will automatically deploy `Nocalhost Dep`, and use the way of ConfigMap to make the deployment configuration.

!!! tip "Using the following commands to try out this demo (trying the Chart package requires ClusterAdmin)"

    ```shell
    git clone https://github.com/nocalhost/bookinfo && git checkout config/example
    ```

Then use Helm to install:

```shell
helm dep build ./charts/bookinfo && helm install bookinfo ./charts/bookinfo
```

The content of the deployment configuration itself will not be repeated here. let's take a look at `charts/bookinfo/templates/nocalhost-app-config.yaml`.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  # [Nocalhost]: nocalhost configmap should be named with dev.nocalhost.config.${appName}
  name: "dev.nocalhost.config.{{ .Release.Name }}"
  # [Nocalhost]: labels {dep-management: nocalhost} is also required
  labels:
    dep-management: nocalhost
  annotations:
    "helm.sh/hook": pre-install
data:
  config: |-
    {{ .Files.Get .Values.nocalhost.config.path | nindent 4 }}
```

!!! danger

    This ConfigMap requires to apply to Api Server first. For example, in the Helm application scenario, it should be declared as `pre-install`.

    ```yaml
    annotations:
      "helm.sh/hook": pre-install
    ```

It introduces `.Values.nocalhost.config.path` to render the main body of the deployment configuration. The configuration is actually declared in `./charts/bookinfo/example/config-from-cm/nocalhost-full-config.yaml`, which is a standard Nocalhost deployment configuration without any additional modification:

```yaml
application:
  env:
    - name: APP_ENV_1
      value: EXAMPLE
    - name: APP_ENV_2
      value: EXAMPLE

  services:
    - name: productpage
      serviceType: deployment
      containers:
        - name: productpage
          install:
            env:
              - name: ENV_INJECT_EXAM
                value: BASE

    - name: details
      serviceType: deployment
      containers:
        - install:
            env:
              - name: ENV_INJECT_EXAM
                value: BASE

    - name: ratings
      serviceType: deployment
      dependLabelSelector:
        pods:
          - "productpage"
      containers:
        - install:
            env:
              - name: ENV_INJECT_EXAM
                value: BASE

    - name: reviews
      serviceType: deployment
      dependLabelSelector:
        pods:
          - "productpage"
      containers:
        - install:
            env:
              - name: ENV_INJECT_EXAM
                value: BASE

    - name: authors
      serviceType: deployment
      dependLabelSelector:
        pods:
          - "productpage"
      containers:
        - install:
            env:
              - name: ENV_INJECT_EXAM
                value: BASE
```

## 使用注释进行部署配置

The method of usage is exactly the same as [Which configuration methods Nocalhost supports-place the configuration in annotations](../configure.md#configuration-in-annotations).

!!! danger "Extra attention"

    Since Annotations closely follow the workload, some configurations at the application level are not supported. Only the configurations under `application.services` is supported.

Again, use the same demo project:

!!! tip "Use the following commands to get and try out this project (trying the Chart package requires ClusterAdmin)"

    ```shell
    git clone https://github.com/nocalhost/bookinfo && git checkout config/example
    ```

Then use Helm to install:

```shell
helm dep build ./charts/bookinfo && helm install bookinfo ./charts/bookinfo -f ./charts/bookinfo/values-annotation-config.yaml
```

We specified `values-annotation-config.yaml` as Values.yaml, which specifies the rendering of the local configuration file to the Annotations of the workload. Take `./charts/bookinfo/templates/microservice-details.yaml` as an !!! example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: details
  {{- if .Values.nocalhost.annotations.path.details }}
  annotations:
    nocalhost.dev: |-
      {{ .Files.Get .Values.nocalhost.annotations.path.details | nindent 6 }}
  {{- end }}
  labels:
    app: details
```

Helm will render the path configured by `.Values.nocalhost.annotations.path.details` into yaml, whose content is as follows, that is, `./charts/bookinfo/example/config-from-annotations/details.yaml` specified in Values :

```yaml
containers:
  - install:
      env:
        - name: ENV_INJECT_EXAM
          value: ANNOTATIONS
```

## 如何部署 `Nocalhost Dep`

我们建议使用`Nocalhost Server`获取`Nocalhost Dep`的所有功能。

!!! danger

    即将推出
