---
title: nhctl get
---

Get resource info

## 用法

```
nhctl get type [flags]
```

```
!!! example

# Get all application
nhctl get application --kubeconfig=kubeconfigfile

# Get all application in namespace
nhctl get application -n namespaceName --kubeconfig=kubeoconfigpath

# Get all deployment of application in namespace
nhctl get deployment -n namespaceName -a bookinfo --kubeconfig=kubeconfigpath
```

## 标志

```
Flags:
  -a, --application string        application name
  -h, --help                      help for get
  -o, --outputType string         json or yaml
  -l, --selector stringToString   Selector (label query) to filter on, only supports '='.(e.g. -l key1=value1,key2=value2) (default [])

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```
