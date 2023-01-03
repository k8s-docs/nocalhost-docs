# Upgrade

Upgrade nocalhost server whit [Helm](https://helm.sh/docs/intro/install/)

## Prerequisites

- Helm 3+

## Add Helm Chart Repository

```console
helm repo add nocalhost "https://nocalhost-helm.pkg.coding.net/nocalhost/nocalhost"
helm repo update
```

## 升级

```console
helm upgrade nocalhost nocalhost/nocalhost -n nocalhost
```

!!! tip

    升级nocalhost时，请使用标志`--reset-values`, 如果您使用`nhctl init`安装Nocalhost服务器
