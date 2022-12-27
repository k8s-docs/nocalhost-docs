---
title: nhctl init
---

Init demo or dep component

```
Usage:
  nhctl init [command]

Available Commands:
  demo        Init Nocalhost with demo mode
  dep         dep component
```

## nhctl init demo

Init api, web and dep component in cluster

### Usage

```
nhctl init demo [flags]
```

```
Example:

nhctl init demo -n [DevSpace Name] -t nodeport -p [port]
nhctl init demo -n [DevSpace Name]
```

### Flags

```
Flags:
      --force                         force to init, warning: it will remove all nocalhost old data
  -h, --help                          help for demo
      --inject-user-amount int        inject user amount, example 10, max is 999
      --inject-user-offset int        inject user id offset, default is 1 (default 1)
      --inject-user-template string   inject users template, example Techo%d, max length is 15
  -p, --port int                      for NodePort usage set ports (default 80)
      --set strings                   set values of helm
  -s, --source string                 (Deprecated) bookinfo source, github or coding, default is github
  -t, --type string                   set NodePort or LoadBalancer to expose nocalhost service

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```

## nhctl init dep

Nocalhost（nocalhost-dep）组件初始化

### Usage

```
nhctl init dep [flags]
```

### Flags

```
Flags:
  -h, --help   help for dep

Global Flags:
      --debug               enable debug level log
      --kubeconfig string   the path of the kubeconfig file
  -n, --namespace string    kubernetes namespace
```
