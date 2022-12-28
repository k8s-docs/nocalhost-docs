---
title: nhctl yaml
---

Yaml tools

```
Usage:
  nhctl yaml [command]

Available Commands:
  from-json   Convert json to yaml
  to-json     Convert yaml to json
```

## nhctl yaml from-json

Convert json to yaml

### 用法

```
nhctl yaml from-json [flags]
```

### 标志

```
Flags:
  -h, --help   help for from-json

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```

## nhctl yaml to-json

Convert yaml to json

### 用法

```
nhctl yaml to-json [flags]
```

### 标志

```
Flags:
  -h, --help   help for yaml

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```
