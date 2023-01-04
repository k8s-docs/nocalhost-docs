# 安装 Nocalhost

!!! tip "nhctl"

    安装Nocalhost IDE插件时，它将自动为您安装`nhctl`。

## 兼容的

<table>
    <thead>
        <tr>
            <th>IDE</th>
            <th>Version</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>VS Code </td>
            <td>1.58.2 (Universal)</td>
            <td><strong className="pass-tag"passed</strong></td>
        </tr>
        <tr>
            <td rowspan="3" >JetBrains</td>
            <td>2021.2 - Intel and Apple Silicon</td>
            <td><strong className="pass-tag"passed</strong></td>
        </tr>
        <tr>
            <td>2021.1 - Intel and Apple Silicon</td>
            <td><strong className="pass-tag"passed</strong></td>
        </tr>
        <tr>
            <td>2020.3 - Intel and Apple Silicon</td>
            <td><strong className="pass-tag"passed</strong></td>
        </tr>
    </tbody>
</table>

## 安装 VS 代码插件

=== "插件市场"

    1. 打开VS Code，点击 <img src='../img/icons/vs-code-icon.jpg' width="20" /> 图标，进入`Extensions`
    2. 在搜索框中输入`Nocalhost`
    3. 选择`Nocalhost Extension`，然后点击 **Install** 按钮。

    ![从VS代码扩展市场安装](./img/installation/vscode-market.png)

=== "手动安装"

    1. 从我们的[Github Repo](https://github.com/nocalhost/nocalhost-vscode-plugin/releases/latest)下载最新版本
    2. 打开VS Code，点击<img src='../img/icons/vs-code-icon.jpg' width="20" />图标，进入 `Extensions`
    3. 点击`Extension`列表右上方的<img src='../img/icons/cluster-action-icon.jpg' width="20" />，选择`Install from VSIX…`，选择上文下载的`VSIX`
    4. 从[Github Repo](https://github.com/nocalhost/nocalhost/releases)下载最新的nhctl，并把它放在`~/.nh/bin/`下，然后命名为nhctl，你需要给这个二进制执行权限(`chmod +x ./nhctl`)。
       (windows下的路径是`%homepath%/.nh/bin/`，二进制文件名为`nhctl.exe`，不需要在windows下授予额外的执行权限)

    ![手动安装](./img/installation/vs-manual.jpg)

## 安装 Jetbrains 插件

=== "插件市场"

    #### Windows

    `File > Settings > Plugins > Browse repositories... > Search for "Nocalhost" > Install Plugin`

    #### MacOS

    `Preferences > Settings > Plugins > Browse repositories... > Search for "Nocalhost" > Install Plugin`

    ![从喷气桥扩展市场安装](./img/installation/jb-market.png)

=== "手动安装"

    1. 从我们的[GitHub Repo](https://github.com/nocalhost/nocalhost-intellij-plugin/releases/latest)下载最新版本
    2. 安装插件: <kbd>Preferences</kbd> > <kbd>Plugins</kbd> > <kbd>Install from disk... </kbd>
    3. 从[GitHub Repo](https://github.com/nocalhost/nocalhost/releases)下载最新的NHCTL, 并把它放在 `~/.nh/bin/` 然后命名为 nhctl, 您需要授予此二进制执行权限 (chmod +x ./nhctl). (在Windows中路径是 %homepath%/.nh/bin/ , 和二进制命名 'nhctl.exe', 无需授予Windows下的额外执行权限)

    ![手动安装](./img/installation/jb-manual.jpg)

## 升级插件

IDE 启动时，Nocalhost 将自动检查并安装最新的更新。

## 卸载

您可以通过以下内容完全卸载 nocalhost

### 卸载 IDE 插件

卸载 IDE 中的 Nocalhost IDE 插件

### 删除 `nhctl`

删除根目录中的`.nh`文件夹

=== "mac"

    `.nh` 文件夹在您的`~/`目录中, 您可以通过以下命令将其删除

    ```bash
    rm -rf .nh
    ```

=== "windows"

    `.nh` 文件夹在您的`<ROOT PATH>/User/username/`目录中, 您可以删除它.
