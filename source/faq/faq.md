# 常问问题

## 安装

## 问题

### Linux 的文件同步太慢

如果您的文件同步在 Linux 的速度太慢，则很可能是由于局部限制。

???+ question "如何增加插图限制以使我的文件系统观察者工作？"

    Linux通常限制每个用户的观测量（通常为8192）。当您有更多目录时，您需要调整该数字。

    在许多Linux发行版中，您可以运行以下内容来修复它：

    ```bash
    echo "fs.inotify.max_user_watches=204800" | sudo tee -a /etc/sysctl.conf
    ```

    在Arch Linux和其他可能的其他情况下，首选将此行写入单独的文件，即您应该运行：

    ```bash
    echo "fs.inotify.max_user_watches=204800" | sudo tee -a /etc/sysctl.d/90-override.conf
    ```

    重新启动后，这只会生效。要立即调整限制，请运行：

    ```bash
    sudo sh -c 'echo 204800 > /proc/sys/fs/inotify/max_user_watches'
    ```

## 操作
