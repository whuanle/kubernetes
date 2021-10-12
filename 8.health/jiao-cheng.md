# 教程

## 集群启动问题

当我们使用 kubeadm 创建集群，kubeadm/集群 会在 Linux 的 `systemd` 中开始启动。由于 kubelet 会在每个节点上运行，通过这个程序也可以查看节点的健康状态。

```
systemctl status kubelet.service
```

![1620004583](../.gitbook/assets/1620004583\(1\).png)

kubelet.service 的配置文件可在 `/etc/systemd/system/kubelet.service.d/10-kubeadm.conf` 中找到。

```
...
kubeconfig=/etc/kubernetes/kubelet.conf"
Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml"
...
EnvironmentFile=-/var/lib/kubelet/kubeadm-flags.env
...
```

## 检查 Pod 日志

首先，查看受到影响的容器的日志(正在运行)：

```
kubectl logs ${Pod名称}
kubectl logs ${Pod名称} ${容器名称}
```

如果容器之前崩溃过，可以通过下面命令访问之前容器的崩溃日志：

```
kubectl logs --previous ${Pod名称} ${容器名称}
```

查看事件：

```
kubectl get events --namespace <namespace-name> --sort-by='{.lastTimestamp}'
```

查看 Pod 是否因为资源限制出现问题：

```
kubectl describe quota
kubectl describe quota --namespace <namespace-name>
```

## 调试 Pod

一般容器都会自带 bash 或者 sh 的，所以我们使用 `kubectl exec {pod名称} -- {命令}` 能够在 pod 中执行命令。

`--` 表示前半部分命令已经结束

这里我们可以使用 nginx 进行测试，先部署一个 Nginx：

```
kubectl create deployment nginx --image=nginx:latest
```

查看 pod 列表，然后在 pod 中执行命令

```
kubectl exec -it nginx-55649fd747-7fkh2 -- ls
```

命令格式：

```
kubectl exec ${POD_NAME} -c ${CONTAINER_NAME} -- ${CMD} ${ARG1} ${ARG2} ... ${ARGN}
```

## 临时容器附加调试

但是这里假设，Nginx 这个镜像里面没有 bash 或 sh，ls 、find、cat、vi 这些命令都被移除了，那么我们怎么在 Pod 里面执行命令、修改文件？由于 Pod 是 kubernetes 部署的最小单位，所以在不重新创建 Pod 的情况下，我们是不能为 Pod 增加容器的。

此时可以利用 `kubectl debug` 为已存在的 Pod 附加一个短暂生命的容器。

kubectl-debug 文档地址：[https://github.com/aylei/kubectl-debug/blob/master/docs/zh-cn.md](https://github.com/aylei/kubectl-debug/blob/master/docs/zh-cn.md)

开启 EphemeralContainers 特性：

```
```

## 通过副本调试

为 Pod 创建一个副本，并且进入调试，输入 exit 可退出调试。

```
kubectl debug nginx-55649fd747-7fkh2 -it --copy-to=nginx --container=nginx -- sh
```

## 查看 API-Server 日志

在 master 节点上执行：

```
find / -name *apiserver*log
```

通过日志文件可以检查 api-server 的状态。

或者通过 `kubectl logs` 命令直接查看 pod 的日志：

```
kubectl get pods -n kube-system
```

```
kubectl logs {kube-apiserver名称} -n kube-system
```

## 安装监控的性能度量工具

这里我们将安装一个 metrics-server 工具。

文档可参考：[https://github.com/kubernetes-incubator/metrics-server](https://github.com/kubernetes-incubator/metrics-server)

```
kubectl create -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.7/components.yaml
```

查看 pods，有个 `metrics-server` Pod：

```
kubectl -n kube-system get pod
```

```
NAME                                       READY   STATUS    RESTARTS   AGE
metrics-server-68b849498d-tk6zl            1/1     Running   0          71s
```

修改 deployment

```
kubectl -n kube-system edit deployment metrics-server
```

在 args 后面增加两行参数，最终：

```
    spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-insecure-tls
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
```

查看 日志

```
kubectl -n kube-system logs metrics-server-67cd7bd5f6-z7ggh
```

```
I0503 03:03:10.999187       1 serving.go:312] Generated self-signed cert (/tmp/apiserver.crt, /tmp/apiserver.key)
I0503 03:03:11.416552       1 secure_serving.go:116] Serving securely on [::]:4443
```
