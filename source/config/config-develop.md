---
title: 开发配置
---

# 开发配置

## 一键运行

```yaml
name: example
serviceType: deployment
containers:
  - name: you-container
    dev:
      command:
        run: ["./gradlew", "bootRun"]
```

You can use one-click running after configuring `command.run`. The commands and parameters correspond to different elements in the array. For example, `./gradlew bootRun ` will be `["./gradlew", "bootRun"]`

!!! info "How to use one-click running"

    See more instructions in [Remote Run](../guides/remote-run.md).

## 一键调试

```yaml
name: example
serviceType: deployment
containers:
  - name: you-container
    dev:
      command:
        debug:
          - ./gradlew
          - bootRun
          - --debug-jvm
      debug:
        remoteDebugPort: 5005
```

Apart from configuring `command.debug`, you also need to enter a debug port. For example, the default debug port for gradle is 5005. If you want to use other ports, here `remoteDebugPort` should be changed too.

!!! info "How to use one-click debugging"

    See more instructions in [Remote Debugging](../guides/debug/jetbrains-debug.md).

## 配置热重载

```yaml
name: example
serviceType: deployment
containers:
  - name: you-container
    dev:
      hotReload: true
```

With run or debug configured, you can further configure `hotReload: true` to enable HotReload. Nocalhost offers liveReload, so if your programming language and running method do not support HotReload or the configuration is too complex, you can try to use the HotReload provided by Nocalhost.

!!! example "HotReload with run command"

    ```yaml
    name: example
    serviceType: deployment
    containers:
      - name: you-container
        dev:
          command:
            run: ["./gradlew", "bootRun"]
          hotReload: true
    ```
