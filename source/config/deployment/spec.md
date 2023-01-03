---
title: Spec
---

!!! danger "一些配置需要其他组件"

    部署配置的某些功能取决于 **Nocalhost-Dep 组件**。
    如果您使用Nocalhost服务器，我们将为您自动安装此组件。
    否则需要额外的安装。

    如果未安装**Nocalhost-Dep component**，则将限制一些功能。
    本文将标记需要**Nocalhost-Dep**的功能.

## 启动依赖性控制 ([组件依赖性](#danger))

!!! example

    ```yaml
    application:
      name: example
      manifestType: rawManifestGit
      resourcePath: ["example"]

      services:
        - name: whatever
          serviceType: deployment

          dependLabelSelector:
            pods:
              - "name=mariadb"
              - "app.kubernetes.io/name=example"
            jobs:
              - "job-name=init-job"
    ```

当资源在部署配置中声明`dependLabelSelector`时, `pods`数组中的字符表示要等待的 Pod 的标签.
格式是键值对。`jobs`数组中的字符代表要等待的 JOD 的标签。
格式也是键值对。

`pods` and `jobs`都是可选的。
当您实际部署应用程序时, 它将为指定的工作负载生成一个`initContainer`, 等待所有与标签匹配的 POD 的状态`Ready`, 并等待与标签相匹配的所有工作状态`Succeeded`.

## 注射环境变量 ([组件依赖性](#danger))

### 注入全局变量

!!! example

    ```yaml
    application:
      name: example
      manifestType: rawManifestGit
      resourcePath: ["example"]

      ##### start
      env:
        - name: DEBUG
          value: false
        - name: DOMAIN
          value: nocalhost.dev
      envFrom:
        envFile:
          - path: relpath/to/env/file
      ##### end
    ```

Injecting global variables needs to be declared at the level of `application`. During the deployment, it will inject the corresponding environment variables into all deployed `Deployment`, `DaemonSet`, `ReplicaSet`, `StatefulSet`, `Job`, and `CronJob`.

!!! tip "Injecting variables supports two kinds syntax, which can be mixed"

    - The first one is to declare key-value pairs directly
    - The second is to declare an env file relative to `config.yaml`, the content is line-by-line `Key=Value`:

    ```dotenv
    DEBUG=true
    DOMAIN=nocalhost.dev
    ```

The priority of `env` is higher than that of `envFrom`

### 在容器级别注入变量

!!! example

    ```yaml
    application:
      name: example
      manifestType: rawManifestGit
      resourcePath: ["example"]

      services:
        - name: whatever
          serviceType: deployment
          containers:
            - name: your-container-name
              install:
                ##### start
                env:
                  - name: DEBUG
                    value: false
                  - name: DOMAIN
                    value: nocalhost.dev
                envFrom:
                  envFile:
                    - path: relpath/to/env/file
                ##### end
    ```

The container-level variable injection is declared in `application.services[*].containers[*].install`, indicating that the corresponding variables are injected into the corresponding container during deployment. The rules of `env` and `envFrom` are in line with the application level's.

## 安装后自动端口转发

!!! example

    ```yaml
    application:
      name: example
      manifestType: rawManifestGit
      resourcePath: ["example"]

      services:
        - name: whatever
          serviceType: deployment
          containers:
            - name: your-container-name
              install:
                ##### start
                portForward:
                  - 5005:5005
                  - 3306:3306
                ##### end
    ```

The configuration rules are similar to the container and variable injection declarations, and need to be configured in `application.services[*].containers[*].install`.

The port forwarding after installation is as it's name implies. After the application is installed, it can automatically forward port to the local for some services. For instance, database, MQ and other commonly used services' ports are suitable for automatic forwarding and configuration rules after installation. The port forwarding rules are consistent with K8s, namely `local port: container port`.

## 钩

!!! example

    ```yaml
    application:
      name: example
      manifestType: rawManifestGit
      resourcePath: ["example"]

      ##### start
      onPreInstall:
        - path: manifest/templates/pre-install/print-num-job-01.yaml
          weight: "0"
        - path: manifest/templates/hook/pre-install.yaml
          weight: "1"
      onPostInstall:
        - path: manifest/templates/hook/post-install.yaml
          weight: "1"
      onPreUpgrade:
        - path: manifest/templates/hook/pre-upgrade.yaml
          weight: "1"
      onPostUpgrade:
        - path: manifest/templates/hook/post-upgrade.yaml
          weight: "1"
      onPreDelete:
        - path: manifest/templates/hook/pre-delete.yaml
          weight: "1"
      onPostDelete:
        - path: manifest/templates/hook/post-delete.yaml
          weight: "1"
      ##### end
    ```

Nocalhost supports injecting various hooks in the life cycle of the application. **Hooks currently only support Job**, where `path` is the RawManifest **relative to the home directory**, which must be the job type. `weight` is the weight. The lower ones are executed first.

!!! danger "The limits on the Hook"

    Hook is similar to Helm's Hook. Hook itself is to make up for the shortcomings of non-Helm applications, so ** Helm-type applications cannot configure Hook (You can use Helm's Hook directly)**.

!!! info "The explanation of Hook"

    - `onPreInstall` occurs before the employment of the application, including performing some initialization operations such as clusters and databases. The deployment will start after the job status is `Successed`. If it fails, the installation will be terminated.
    - `onPostInstall` occurs after the application is deployed. When all resources are submitted to the K8s Api Server, this job will be executed. After the status is `Successed`, the deployment is successful. Otherwise, it will roll back and perform the uninstall operation.

    By analogy, the Upgrade Hook and Delete Hook will not roll back after the execution fails, and only prompt failure.

## HelmValues 的定制

!!! example

    ```yaml
    application:
      name: example
      manifestType: rawManifestGit
      resourcePath: ["example"]

      ##### start
      helmValues:
        - key: nocalhost.example
          value: foo
        - key: nocalhost.deploy.example
          value: bar

      helmVals:
        nocalhost:
          !!! example foo
          deploy:
            !!! example { { Release.Name } }
      ##### end
    ```

!!! tip "HelmValues 支持两种语法"

    - The first syntax only supports pure strings and has a higher priority.
    - The second syntax is the same as `values.yaml` and can be interspersed with Helm syntax.
