# Kubernetes Commands

## Table of Contents
- [Context and Namespace](#context-and-namespace)
    - [List contexts](#list-contexts)
    - [Show current context](#show-current-context)
    - [Switch context](#switch-context)
    - [Set default namespace for current context](#set-default-namespace)
- [Pod](#pod)
    - [List Pods](#list-pods)
    - [List Pods on a specific node](#list-pods-on-node)
    - [Check the state of a Pod's containers](#check-pod-container-state)
    - [Show Pod YAML](#show-pod-yaml)
    - [Delete a Pod](#delete-pod)
- [Logs](#logs)
    - [View logs](#view-logs)
    - [Stream logs](#stream-logs)
    - [View logs for a specific container](#logs-specific-container)
    - [View previous container logs](#previous-logs)
- [Exec](#exec)
    - [Open a shell in a Pod](#open-shell-in-pod)
    - [Run a single command in a Pod](#run-command-in-pod)
- [Deployment](#deployment)
    - [List Deployments](#list-deployments)
    - [Describe a Deployment](#describe-deployment)
    - [Restart a Deployment](#restart-deployment)
    - [Scale a Deployment](#scale-deployment)
    - [Check rollout status](#check-rollout-status)
    - [View rollout history](#view-rollout-history)
- [Service](#service)
    - [List Services](#list-services)
    - [Describe a Service](#describe-service)
    - [Port-forward to a Service](#port-forward-service)
- [Node](#node)
    - [List Nodes](#list-nodes)
    - [Describe a Node](#describe-node)
- [Resources](#resources)
    - [Get all common resources in a namespace](#get-all-resources)
    - [Show resource usage](#show-resource-usage)
- [Manifest](#manifest)
    - [Apply a manifest](#apply-manifest)
    - [Delete resources from a manifest](#delete-manifest)
    - [Diff a manifest](#diff-manifest)

## Context and Namespace <a name="context-and-namespace"></a>
### List contexts <a name="list-contexts"></a>
```console
$ kubectl config get-contexts
```

### Show current context <a name="show-current-context"></a>
```console
$ kubectl config current-context
```

### Switch context <a name="switch-context"></a>
```console
$ kubectl config use-context <context-name>
```

### Set default namespace for current context <a name="set-default-namespace"></a>
```console
$ kubectl config set-context --current --namespace=<namespace>
```

## Pod <a name="pod"></a>
### List Pods <a name="list-pods"></a>
```console
$ kubectl get pods
```

看更多信息可以用:
```console
$ kubectl get pods -o wide
```

All namespaces:
```console
$ kubectl get pods -A
```

### List Pods on a specific node <a name="list-pods-on-node"></a>
```console
$ kubectl get pods -A -o wide --field-selector spec.nodeName=<node-name>
```

### Check the state of a Pod's containers <a name="check-pod-container-state"></a>
To check the state of a Pod's containers, use:
```console
$ kubectl describe pod <name-of-pod>
```

如果 Pod 不在 default namespace, 可以加 `-n`:
```console
$ kubectl describe pod <name-of-pod> -n <namespace>
```

### Show Pod YAML <a name="show-pod-yaml"></a>
```console
$ kubectl get pod <name-of-pod> -o yaml
```

### Delete a Pod <a name="delete-pod"></a>
```console
$ kubectl delete pod <name-of-pod>
```

## Logs <a name="logs"></a>
### View logs <a name="view-logs"></a>
```console
$ kubectl logs <pod-name>
```

### Stream logs <a name="stream-logs"></a>
```console
$ kubectl logs -f <pod-name>
```

### View logs for a specific container <a name="logs-specific-container"></a>
一个 Pod 里有多个 container 时，用 `-c` 指定:
```console
$ kubectl logs <pod-name> -c <container-name>
```

### View previous container logs <a name="previous-logs"></a>
container restart 过以后，上一轮日志可以这样看:
```console
$ kubectl logs <pod-name> --previous
```

## Exec <a name="exec"></a>
### Open a shell in a Pod <a name="open-shell-in-pod"></a>
```console
$ kubectl exec -it <pod-name> -- /bin/sh
```

如果 image 里有 bash:
```console
$ kubectl exec -it <pod-name> -- /bin/bash
```

### Run a single command in a Pod <a name="run-command-in-pod"></a>
```console
$ kubectl exec <pod-name> -- env
```

## Deployment <a name="deployment"></a>
### List Deployments <a name="list-deployments"></a>
```console
$ kubectl get deployments
```

### Describe a Deployment <a name="describe-deployment"></a>
```console
$ kubectl describe deployment <deployment-name>
```

### Restart a Deployment <a name="restart-deployment"></a>
```console
$ kubectl rollout restart deployment/<deployment-name>
```

### Scale a Deployment <a name="scale-deployment"></a>
```console
$ kubectl scale deployment <deployment-name> --replicas=<count>
```

### Check rollout status <a name="check-rollout-status"></a>
```console
$ kubectl rollout status deployment/<deployment-name>
```

### View rollout history <a name="view-rollout-history"></a>
```console
$ kubectl rollout history deployment/<deployment-name>
```

## Service <a name="service"></a>
### List Services <a name="list-services"></a>
```console
$ kubectl get svc
```

### Describe a Service <a name="describe-service"></a>
```console
$ kubectl describe svc <service-name>
```

### Port-forward to a Service <a name="port-forward-service"></a>
```console
$ kubectl port-forward svc/<service-name> 8080:80
```

## Node <a name="node"></a>
### List Nodes <a name="list-nodes"></a>
```console
$ kubectl get nodes
```

### Describe a Node <a name="describe-node"></a>
```console
$ kubectl describe node <node-name>
```

## Resources <a name="resources"></a>
### Get all common resources in a namespace <a name="get-all-resources"></a>
```console
$ kubectl get all
```

### Show resource usage <a name="show-resource-usage"></a>
Node:
```console
$ kubectl top nodes
```

Pod:
```console
$ kubectl top pods
```

## Manifest <a name="manifest"></a>
### Apply a manifest <a name="apply-manifest"></a>
```console
$ kubectl apply -f <file>.yaml
```

### Delete resources from a manifest <a name="delete-manifest"></a>
```console
$ kubectl delete -f <file>.yaml
```

### Diff a manifest <a name="diff-manifest"></a>
```console
$ kubectl diff -f <file>.yaml
```
