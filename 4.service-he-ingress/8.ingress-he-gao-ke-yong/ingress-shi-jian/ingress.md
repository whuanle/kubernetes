# ingress

## 前提

已经安装了 ingress controller，并启动了 ingress 控制器。

## 创建 Ingress

创建 ingress 资源：

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: test-ingress
spec:
  defaultBackend:
    service:
      name: web1
      port:
        number: 8080
```

```text
kubectl apply -f ingress.yaml
```

执行命令后需要等待一段时间。

在主机的 `/etc/hosts` 文件中，加上

```text
10.102.149.37 hello-world.info
```

这个 ip 为前面查询的 Service 的 ip。

