---
title: 增强配置
---

# 增强配置

## 在开发模式下自动端口转发

```yaml
name: nocalhost-api
serviceType: deployment
containers:
  - name: nocalhost-api
    dev:
      portForward:
        - 8080:80
        - 3306:3306
```

如果您希望进入 Dev Mode 后，Dev Container 的某些端口可以自动端口转发到本地，可以进行相关配置。

!!! danger "关于系统端口"

    如果需要在本地端口(例如Ubuntu或Windows的系统端口(小于1024))上进行监听，则进入Dev Mode后无法启用自动端口转发功能。

## 源代码地址

```yaml
name: nocalhost-api
serviceType: deployment
containers:
  - name: nocalhost-api
    dev:
      gitUrl: git@github.com:nocalhost/nocalhost.git
```

源代码地址指服务的 git 源代码目录。
它用于快速和轻松地下载源代码，它支持 http/https 和 ssh 协议。

!!! tip

    能否成功克隆代码取决于设备是否有权限。

最好提前为每个服务配置 gitUrl。这样可以大大降低团队之间的沟通成本。

## 文件同步

```yaml
name: nocalhost-api
serviceType: deployment
containers:
  - name: nocalhost-api
    dev:
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

The configuration of file synchronization includes three parts:

- The first is the synchronization type, which includes:

!!! note

    - send. Send only (one-way). All changes depend on the local, and the remote changes will not affect the local (by default).
    - sendReceive. Send and receive (two-way). Operations such as adding, modifying, and deleting at either end will be synchronized to the other end.

- The second is the local file deletion protection `containers[].dev.sync.deleteProtection`, which is a Bool value.

  - If this function is enabled, the deletion in the remote will not be synchronized to the local (this function is enabled by default).
  - If this function is disabled when `sendReceive` is enabled, the file will be deleted in the local when it is deleted in the remote.

- The third is synchronization mode. You are required to select a local directory when entering Dev Mode, and then Nocalhost will synchronize all files under this directory by default. If you do not want to synchronize all files, you can customize it.

Nocalhost offers two synchronization modes. `containers[].dev.sync.mode`

!!! note

    - pattern. Synchronize files based on pattern matching (default mode).
    - gitIgnore. Synchronize and ignore files according to `gitIgnore`.

### 模式模式

If you want to use pattern mode, you can configure filePattern and ignoreFilePattern to customize, such as only synchronizing building products or ignoring all files irrelevant to the building.

As the example given above, it means to synchronize files in send only way and ignore `.git`、`.github`、`.vscode` and `node_modules`.

!!! info "Pattern"

    See more instructions in [Pattern Config](config-pattern.md).

### Gitignore 模式

This mode is easy to use. It will automatically use `git` ignore configuration, such as `gitignore `.

!!! warning "Requirements"

    Since this function is based on `git`, you need to make sure that your device has installed `git`. Moreover, the synchronized directory has to be in a `git` project.

    If the above two requirements are not met, Nocalhost will not enable synchronization control, which means that all files will be synchronized. This is equivalent to no synchronization configuration.

!!! tip "Which files are ignored?"

    You can move to the file synchronization directory (e.g., `cd /yourpath`) and then enter `git status --ignored=matching -s` to see the ignored files/folders. Files/folders starting with `!!` will not be synchronized to the remote.

!!! example

    ```yaml
    name: nocalhost-api
    serviceType: deployment
    containers:
      - name: nocalhost-api
        dev:
          sync:
            type: send
            mode: gitIgnore
    ```
