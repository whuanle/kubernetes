# 技巧与提示

## 如果使用 VI 编辑器

* 'dG' - 删除从当前位置到文件末尾的内容。
* 'ZZ' ，快速保存文件，或者输入 `:wq!`。

## 切换默认编辑器

在使用 `kubectl edit` 编辑对象时，默认使用的时 VI 编辑器，如果需要修改为 nano 编辑器，可以设置环境变量：

```
export KUBE_EDITOR="nano"
```

kubectl 命令格式：

```
kubectl [command] [type] [Name] [flag]
```

| **all**                                             | **events** (**ev**)                    | **podsecuritypolicies** (**psp**)   |
| --------------------------------------------------- | -------------------------------------- | ----------------------------------- |
| **certificatesigningrequests** (**csr**)            | **horizontalpodautoscalers** (**hpa**) | **podtemplates**                    |
| **clusterrolebindings**                             | **ingresses** (**ing**)                | **replicasets** (**rs**)            |
| **clusterroles**                                    | **jobs**                               | **replicationcontrollers** (**rc**) |
| **clusters** (valid only for federation apiservers) | **limitranges** (**limits**)           | **resourcequotas** (**quota**)      |
| **componentstatuses** (**cs**)                      | **namespaces** (**ns**)                | **rolebindings**                    |
| **configmaps** (**cm**)                             | **networkpolicies** (**netpol**)       | **roles**                           |
| **controllerrevisions**                             | **nodes** (**no**)                     | **secrets**                         |
| **cronjobs**                                        | **persistentvolumeclaims** (**pvc**)   | **serviceaccounts** (**sa**)        |
| **customresourcedefinition** (**crd**)              | **persistentvolumes** (**pv**)         | **services** (**svc**)              |
| **daemonsets** (**ds**)                             | **poddisruptionbudgets** (**pdb**)     | **statefulsets**                    |
| **deployments** (**deploy**)                        | **podpreset**                          | **storageclasses**                  |
| **endpoints** (**ep**)                              | **pods** (**po**)                      |                                     |
