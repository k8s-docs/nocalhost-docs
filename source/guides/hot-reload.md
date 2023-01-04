---
title: 热重载
---

## 什么是热重载？

您在 IDE 中修改的文件将实时同步到远程容器，并且您的运行/调试命令将重新执行。

<iframe width="100%" height="600" src="//player.bilibili.com/player.html?aid=335688273&bvid=BV1sR4y1p7RM&cid=415232002&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## 支持的 IDE

<table>
  <tbody>
    <tr>
      <th>Language</th>
      <th>IDE</th>
      <th>Edition</th>
      <th>Required Plugin</th>
    </tr>
    <tr>
      <td>Java</td>
      <td>IntelliJ IDEA</td>
      <td>Ultimate</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td rowSpan="2">Go</td>
      <td>IntelliJ IDEA</td>
      <td>Ultimate</td>
      <td>Go plugin</td>
    </tr>
    <tr>
      <td>GoLand</td>
      <td>Professional</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td rowSpan="2">Python</td>
      <td>IntelliJ IDEA</td>
      <td>Ultimate</td>
      <td>Python plugin</td>
    </tr>
    <tr>
      <td>PyCharm</td>
      <td>Professional</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td rowSpan="2">PHP</td>
      <td>IntelliJ IDEA</td>
      <td>Ultimate</td>
      <td>PHP plugin</td>
    </tr>
    <tr>
      <td>PhpStorm</td>
      <td>Professional</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td rowSpan="2">Node.js</td>
      <td>IntelliJ IDEA</td>
      <td>Ultimate</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td>WebStorm</td>
      <td>Professional</td>
      <td>N/A</td>
    </tr>
  </tbody>
</table>

## 如何启用热重载？

1. 选择要运行/调试的工作负载
2. 右键单击工作量并选择 **`Dev Config`**, 配置 **`hotReload: true`**

### 示例配置

```yaml title="Nocalhost Configs"
name: java-remote-run
serviceType: deployment
containers:
  - name: "reviews"
    dev:
      image: codingcorp-docker.pkg.coding.net/nocalhost/dev-images/java:latest
      shell: bash
      workDir: /home/nocalhost-dev
      command:
        debug:
          - ./gradlew
          - bootRun
          - --debug-jvm
          - --no-daemon
      hotReload: true
      debug:
        remoteDebugPort: 5005
```
