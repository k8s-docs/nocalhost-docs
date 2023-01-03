---
title: Spec
---

!!! info "在开始本节之前，请确保您已经知道如何配置 Nocalhost。 如果没有，请先阅读[配置概述](config-overview-en.md)。"

Nocalhost 的配置可以分为三个部分。

## 1. [开发容器配置](config-dev-container.md)

```yaml title="第一部分是开发容器配置，包括："
name: nocalhost-api
serviceType: deployment
containers:
  - name: nocalhost-api
    dev:
      # 1. 开发镜像
      image: codingcorp-docker.pkg.coding.net/nocalhost/dev-images/golang:zsh
      # 2. 文件同步的远程目录
      workDir: /home/nocalhost-dev
      # 3. 开发容器的默认 shell
      shell: /bin/zsh
      # 4. 开发容器的持久性（卷）
      persistentVolumeDirs:
        - path: /the/path/you/want/to/persistent/in/container
          capacity: 10Gi
        - path: /other
          capacity: 10Gi
      storageClass: cbs
      # 5. 开发容器资源的请求和限制
      resources:
        limits:
          memory: 4Gi
          cpu: "1"
        requests:
          memory: 2Gi
          cpu: "0.5"
      # 6. SideCar 镜像自定义
      sidecarImage: nocalhost-docker.pkg.coding.net/nocalhost/public/nocalhost-sidecar:sshversion
```

## 2. [增强配置](config-enhance.md)

```yaml title="第二部分是增强配置，该配置独立于开发容器，包括："
name: nocalhost-api
serviceType: deployment
containers:
  - name: nocalhost-api
    dev:
      # 1. GIT 的源代码目录
      gitUrl: git@github.com:nocalhost/nocalhost.git
      # 2. 进入`DevMode`后，是否会自动打开端口转发
      portForward:
        - 8080:80
        - 3306:3306
      # 3. 文件同步配置，包括同步模式和忽略模式
      sync:
        type: send
        mode: pattern
        filePattern:
          - .
        ignoreFilePattern:
          - ".git"
          - ".github"
          - ".vscode"
          - "node_modules"
```

## 3. [运行、调试及热重载配置](config-develop.md)

```yaml title="第三部分是开发过程的配置，包括："
name: example
serviceType: deployment
containers:
  - name: you-container
    dev:
      command:
        # 1. 一键运行
        run: ["./gradlew", "bootRun"]
        # 2. 一键调试
        debug: ["./gradlew", "bootRun", "--debug-jvm"]
      debug:
        remoteDebugPort: 5005
      # 3. 热重载
      hotReload: true
```
