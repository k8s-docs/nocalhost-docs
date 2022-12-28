---
title: nhctl daemon
---

nhctl daemon

```
Usage:
nhctl daemon [command]

Available Commands:
  info        Get nhctl daemon info
  restart     Restart nhctl daemon
  start       Start nhctl daemon
  status      Get nhctl daemon status
  stop        Stop nhctl daemon
```

## nhctl daemon info

### 用法

```
nhctl daemon info
```

```
!!! example

> nhctl daemon info

> {"Version":"v0.4.12","CommitId":"0f02d7f90335076b502bca3f40ff42bd37ee55e6","NhctlPath":".nh/bin/nhctl","Upgrading":false}
```

### 标志

```
Flags:
  -h, --help   help for restart
      --sudo   Is run as sudo

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```

## nhctl daemon restart

重启 nhctl daemon

### 用法

```
nhctl daemon restart
```

```
!!! example

> nhctl daemon restart
> RestartDaemonServerCommand has been sent
```

### 标志

```
Flags:
  -h, --help   help for restart
      --sudo   Is run as sudo

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```

## nhctl daemon start

Manually start nhctl daemon

### 用法

```
nhctl daemon start [flags]
```

### 标志

```
Flags:
  -d, --daemon   Is run as daemon(background)
  -h, --help     help for start
      --sudo     Is run as sudo

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```

## nhctl daemon status

View nhctl daemon status

### 用法

```
nhctl daemon status
```

```
!!! example

> nhctl daemon status
> {"portForwardList":[]}
```

### 标志

```
Flags:
  -h, --help   help for status
      --sudo   Is run as sudo

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```

## nhctl daemon stop

Manually stop nhctl daemon

### 用法

```
nhctl daemon stop
```

### 标志

```
Flags:
  -h, --help   help for stop
      --sudo   Is run as sudo

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```
