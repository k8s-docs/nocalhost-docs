---
title: nhctl pvc
---

Manage PersistVolumeClaims

```
Usage:
  nhctl pvc [command]

Available Commands:
  clean       Clean up PersistVolumeClaims
  list        List PersistVolumeClaims
```

## nhctl pvc clean

Clean up PersistVolumeClaims

### 用法

```
nhctl pvc clean [flags]
```

### 标志

```
Flags:
      --app string          Clean up PVCs of specified application
      --controller string   Clean up PVCs of specified service
  -h, --help                help for clean
      --name string         Clean up specified PVC

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```

## nhctl pvc list

List PersistVolumeClaims

### 用法

```
nhctl pvc list [flags]
```

### 标志

```
Flags:
      --app string   List PVCs of specified application
  -h, --help         help for list
      --json         Use json as the output format
      --svc string   List PVCs of specified service
      --yaml         Use yaml as the output format

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```
