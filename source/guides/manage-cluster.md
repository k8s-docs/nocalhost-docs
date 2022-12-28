# Manage Cluster

## 连接到 Kubernetes 群集

Nocalhost 支持多集群管理，您可以使用两种方法连接到 Kubernetes 群集：

**由 kubeconfig 连接**

从任何本地目录中选择`KubeConfig`文件。

!!! tip "Default KubeConfig"

    Nocalhost will try to load `KubeConfig` from your local `~/.kube/config` by default.

**将 kubeconfig 粘贴为文本**

Paste the `KubeConfig` as a text.

!!! tip "Get KubeConfig"

    You can use the following command to view your `KubeConfig`, copy and paste to the Nocalhost plugin.

    ```bash
    kubectl config view --minify --raw --flatten
    ```

=== "vscode"

    ![Connect to cluster in VS Code](../img/opt/vscode-add-cluster.gif)

=== "jet"

    ![Connect to cluster in JetBrains IDE](../img/opt/idea-connect-cluster.gif)

## Remove Cluster

!!! note "KubeConfig Unchanged"

    Nocalhost will only remove the cluster from inspector, it will not modify your `KubeConfig`.

=== "vscode"

    ![Remove cluster in VS Code](../img/opt/vscode-remove-cluster.gif)

=== "jet"

    ![Remove cluster in JetBrains IDE](../img/opt/idea-remove-cluster.gif)

## View KubeConfig

Right-click the specified cluster and select `View KubeConfig`, the Nocalhost plugin will open the `KubeConfig` of the cluster.

=== "vscode"

    ![View KubeConfig in VS Code](../img/opt/vscode-view-config.gif)

=== "jet"

    ![View KubeConfig in JetBrains IDE](../img/opt/idea-view-config.gif)
