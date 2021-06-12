# 实践

创建 nginx pod

```text
kubectl create deployment nginx --image=nginx:latest
```

创建service

```text
kind: Service
apiVersion: v1
metadata:
  name: nginx-svc
spec:
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  selector:
    app: nginx
```

创建 ingress 服务

```text
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ingress-demo
spec:
  backend:
    serviceName: nginx-svc
    servicePort: 80
```

通过 80 端口即可访问。

