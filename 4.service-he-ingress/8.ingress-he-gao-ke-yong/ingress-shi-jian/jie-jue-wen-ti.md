# 解决问题

如果出现

```text
Error from server (InternalError): error when creating "ingress.yaml": Internal error occurred: failed calling webhook "validate.nginx.ingress.kubernetes.io": Post "https://ingress-nginx-controller-admission.ingress-nginx.svc:443/networking/v1beta1/ingresses?timeout=10s": dial tcp 10.98.246.133:443: i/o timeout
```

使用下面的命令查看 webhook

```text
kubectl get validatingwebhookconfigurations
```

```text
NAME                      WEBHOOKS   AGE
ingress-nginx-admission   1          4h5m
```

删除ingress-nginx-admission kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission

