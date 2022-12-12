## K8S 快捷命令

Kuberentes 提供了 `kubectl explain` 命令，可以帮助我们快速了解创建一个对象的 YAML 需要哪几部分内容。例如创建 Pod 的 YAML 定义如下：

```bash
oot@master:~# kubectl explain pod
KIND:     Pod
VERSION:  v1

DESCRIPTION:
     Pod is a collection of containers that can run on a host. This resource is
     created by clients and scheduled onto hosts.

FIELDS:
   apiVersion	<string>
     APIVersion defines the versioned schema of this representation of an
     object. Servers should convert recognized schemas to the latest internal
     value, and may reject unrecognized values. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

   kind	<string>
     Kind is a string value representing the REST resource this object
     represents. Servers may infer this from the endpoint the client submits
     requests to. Cannot be updated. In CamelCase. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

   metadata	<Object>
     Standard object's metadata. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

   spec	<Object>
     Specification of the desired behavior of the pod. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

   status	<Object>
     Most recently observed status of the pod. This data may not be up to date.
     Populated by the system. Read-only. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
```

如果要查更深一层的字段，则可以：

```bash
root@master:~# kubectl explain pod.kind
KIND:     Pod
VERSION:  v1

FIELD:    kind <string>

DESCRIPTION:
     Kind is a string value representing the REST resource this object
     represents. Servers may infer this from the endpoint the client submits
     requests to. Cannot be updated. In CamelCase. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds
```

> **[Info] 提示**
>
> 此方式查询到的文档，其列举的字段不一定都是 Pod YAML 所需要的，还需要读者多练习多学习，从文档中补充需要的信息。





**使用 Json 表达式筛选字段**

如果是单个对象，例如 Pod test，那么获取其命名空间：

```bash
kubectl get pod test -o=jsonpath="{.metadata.namespace}"
```

如果是多个对象，则需要使用 `.item[*]`。

```bash
kubectl get pods -o=jsonpath="{.item[*].metadata.namespace}"
```

如果要获取多个字段：

```bash
kubectl get pods -o=jsonpath="{.items[*]['metadata.name', 'metadata.namespace']}"
```

查看 Pod 的容器镜像列表：

```bash
kubectl get po nginx -o jsonpath='{.spec.containers[].image}{"\n"}'
```



获取 skywalker 命名空间下带有 `jedi=true` 标签的 Pod 的名称和镜像名称，输出格式：`podname,imagename`。

```bash
kubectl get po  -n skywalker --selector=jedi=true -o jsonpath="{range .items[*]}{.metadata.name},{.spec.containers[0].image}{'\n'}{end}" 
```





**创建 Pod**

在旧版本的 kubectl 中，使用 `kubectl run` 可以创建 Pod 和 Deployment 等，但是前面不需要加上资源类型，而是根据后面的参数决定：

```bash
# 创建 Pod
kubectl run nginx --image=nginx --restart=Never
# 创建 Deployment
kubectl run nginx --image=nginx --restart=Alway
```

但是新版本中，为了避免歧义和过多的参数， `kubectl run` 只有一个功能，创建 Pod，所以使用 `kubectl run` 命令，后面不需要加上 `--restart=Never` 了。

其实没必要加，因为有些教材旧了，导致学习者不知情，继续加上去了，我们也跟着加吧。

另外，Pod 没有 `--export` 参数了，`kubectl get po nginx -o yaml --export` 命令是不对的。



使用 YAML 创建 Pod，可以使用下面命令输出 YAML 文件：

```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > nginx-pod.yaml
```

但是，如果是启动一个带命令的 Pod 呢？示例如下：

```bash
kubectl run nginx --image=nginx --restart=Never -o yaml --dry-run=client -- sleep 3600
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - args:
    - sleep
    - "3600"
    image: nginx
    name: nginx
```

如果，要执行一个命令，并且替换容器中默认的启动命令（但是不要这样做，具体看下一条）：

```
kubectl run nginx --image=nginx --restart=Never -o yaml --dry-run=client --command -- sleep 3600
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - command:
    - sleep
    - "3600"
    image: nginx
    name: nginx
```

请留意两个 YAML 的区别，前者是 args，后者是 command。

但是如果要使用自定义命令，一般都是这样的：

```bash
kubectl run busybox --image=busybox --restart=Never --command -o yaml --dry-run=client -- /bin/sh -c "sleep 3600"
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: busybox
  name: busybox
spec:
  containers:
  - command:
    - /bin/sh
    - -c
    - sleep 3600
    image: busybox
    name: busybox
```





强制删除 Pod，在后面加上一个 `--force` 参数即可，这样删除 Pod 时，不需要等待太长时间：

```bash
kubectl delet pod nginx --force
```

如果你连等都不想等，可以加上 `--wait=false`：

```bash
 kubectl delete pod nginx --force --wait=false
```



将 Pod 部署到某个节点：

```
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml
```

然后在 YAML 加上：

```yaml
spec:
  nodeSelector:
    nodeName: nginxnode
```





**镜像更新**

镜像更新版本：

```bash
kubectl set image pod nginx nginx=nginx:1.15-alpine
# kubectl set image pod nginx nginx=nginx:1.15-alpine --record
```

注意，后面的是 `{镜像名称}={镜像名称}:{版本}`，而不是 `--image=nginx:1.15-alpine`。

查看历史版本：

```bash
kubectl rollout history pod nginx
# kubectl rollout history deployment nginx
```

回退上一个版本：

```bash
kubectl rollout undo deployment nginx
```

查看更新状态：

```bash
kubectl rollout status deployment nginx
```

指定回滚版本：

```bash
kubectl rollout history deploy webapp --revision=7
```

暂停、恢复更新：

```bash
kubectl rollout pause deploy webapp
kubectl rollout resume deploy webapp
```





**排序**

使用排序方式获取所有命名空间的 Pod：

```bash
kubectl top pod -A
```

按照 cpu 使用量排序：

```bash
kubectl top pod -A --sort-by=cpu
```

按照 内存 使用量排序：

```bash
kubectl top pod -A --sort-by=memory
```



Pod 按照名称排序：

```bash
kubectl get pods --sort-by=.metadata.name
```

Pod 按照创建时间排序：

```bash
kubectl get pods--sort-by=.metadata.creationTimestamp
```





**执行命令**

以终端的显示执行命令，此时当前终端会进入到 Pod 容器中，所以可以看到容器中的控制台输出：

```bash
kubectl run busybox --image=nginx --restart=Never -it -- echo "How are you"
```

执行命令后自动删除 Pod：

```bash
kubectl run busybox --image=nginx --restart=Never -it --rm -- echo "How are you"
```





**创建多容器 Pod**

Create a Pod with three busy box containers with commands “ls; sleep 3600;”, “echo Hello World; sleep 3600;” and “echo this is the third container; sleep 3600” respectively and check the status。

创建三个使用 busybox 镜像的容器，分别在启动时执行 `ls;sleep 3600`、`echo Hello World;sleep 3600` 、`echo this is the third container; sleep 3600` 。

首先，使用命令导出 YAML：

```bash
kubectl run busybox --image=busybox1 -o yaml --dry-run=client -- /bin/sh -c "ls;sleep 3600"
```

注意，因为要执行命令，所以不能一次性输出三个对应的容器，只能创建一个，然后编辑 YAML 文件。

开始输出：

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - ls;sleep 3600
    image: busybox1
    name: busybox
```

复制 busybox1 的配置，再创建两个容器：

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - ls;sleep 3600
    image: busybox1
    name: busybox
  - args:
    - /bin/sh
    - -c
    - echo Hello World;sleep 3600
    image: busybox2
    name: busybox
  - args:
    - /bin/sh
    - -c
    - echo this is the third container; sleep 3600
    image: busybox3
    name: busybox
```

有些字段是不需要理会的：

```yaml
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```





**查看日志，执行命令**

正常查看日志：

```bash
kubectl logs {pod} -c {容器名称}
```

查看上一个实例的日志：

```bash
kubectl logs {pod} -c {容器名称} --previou
```

容器可能升级过，或者挂了重新部署过等，使用 `--previou` 可以查看此容器上一个实例的日志。



执行命令：

```bash
kubectl exec busybox -c busybox3 -- ls
```



**多容器**

Show metrics of the above pod containers and puts them into the file.log and verify

```bash
kubectl top pod busybox --containers

// putting them into file
kubectl top pod busybox --containers > file.log
cat file.log
```



Create a pod `mp-hello` with image `alpine`,`nginx` and `consul:1.8`. Use command `sleep infinity` for `alpine` container.



可以先使用 `kubectl run mp-hello --image=alpine -o yaml --dry-run=client -- sleep infinity` 命令生成第一个容器，接着复制另外两个容器。

核心部分：

```yaml
  containers:
  - args:
    - sleep
    - infinity
    image: alpine
    name: alpine
  - image: nginx
    name: nginx
  - image: consul:1.8
    name: consul
```



注意，多个镜像，后面的镜像需要带 `-`，否则只会生效最后一个。下面这样是错误的：

```yaml
    image: alpine
    name: alpine
    image: nginx
    name: nginx
    image: consul:1.8
    name: consul
```







**标签**

显示所有标签：

```bash
kubectl get pods --show-labels
```

创建 Pod 时设置标签：

```bash
kubectl run nginx-dev1 --image=nginx --restart=Never --labels=env=dev
```

查找显示 env 标签的值为 dev 的 Pod：

```bash
kubectl get pods -l env=dev
```

查找显示 env 标签的值为 dev 的 Pod，同时输出 Pod 具有的标签：

```bash
kubectl get pods -l env=dev --show-labels
```

具有某个标签的 Pod：

```bash
kubectl get pods -l test --show-labels
```

> 有些文档写成 `kubectl get pods -L env` 是错的，是小写 l，不是大写 L。如果使用 `-L`，会导致没有任何筛选效果。



标签的值是某一种即可：

```bash
kubectl get pods -l 'env in (dev,prod)'
```

覆盖标签的值：

```bash
kubectl label pod/nginx-dev3 env=uat --overwrite
```

移除标签：

```bash
kubectl label pod/nginx-dev3 env-
```

> 增加标签移除标签不需要加上 `-l` 或 `-label`。筛选对象才需要加上  `-l` 或 `-label`。



annotation 也是类似，但是 annotation 的命令关键字是 annotate，而不是 annotation 。

```bash
 kubectl annotate  node node01 flagship-
```





**伸缩**

创建具有多个实例的 Pod：

```bash
kubectl create deployment webapp --image=nginx --replicas=5
```

扩容实例：

```bash
kubectl scale deployment webapp --replicas=5
```



**Job、CronJob**

创建 Job：

```bash
kubectl create job nodeversion --image=node
# kubectl create job nodeversion --image=node -- node -v
```

创建具有表达式的 CronJob：

```bash
kubectl create cronjob date-job --image=busybox --schedule="*/1 * * * *"
```



**PV、PVC**

本节中介绍的只是流程，不同可能记住全部 YAML 的，做题的时候看官方文档复制下载：

Create a hostPath PersistentVolume named task-pv-volume with storage 10Gi, access modes ReadWriteOnce, storageClassName manual, and volume at /mnt/data and verify.

创建一个名称为 task-pv-volume 的 hostPath 类型的 PV 卷，其映射本地目录为 `/mnt/data`，卷空间大小为 10Gi，使用 `ReadWriteOnce` 访问模式，` storageClassName: manual`。

storageClassName 是给卷进行分类，其字段值是自定义的。

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```



声明 PVC，即代表要使用多大的卷：

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: task-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```



挂载到 Pod 中：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: task-pv-pod
spec:
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
        claimName: task-pv-claim
  containers:
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: task-pv-storage
```



**ConfigMap**

创建 configmap，其内容来着键值对：

```bash
kubectl create cm myconfigmap --from-literal=appname=myapp
```

> `--from-literal` 来着键值对，每次只能填写一个键值对，`--from-file` 来自文件或文件夹。

从文件中生成：

```bash
cat >> config.txt << EOF
key1=value1
key2=value2
EOF
```

```bash
kubectl create cm keyvalcfgmap --from-file=config.txt
```



在 Pod 中使用：

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
    envFrom:
    - configMapRef:
        name: keyvalcfgmap
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

主要是：

```bash
    envFrom:
    - configMapRef:
        name: keyvalcfgmap
```

如果只需要一个键：

```bash
    env:
    - name: ENVIRONMENT
      valueFrom:
        configMapKeyRef:
          name: envcfgmap
          key: environment
```



然后是 `--from-env-file`。

Create an env file file.env with var1=val1 and create a configmap envcfgmap from this env file and verify the configmap

```bash
echo var1=val1 > file.env
cat file.env

kubectl create cm envcfgmap --from-env-file=file.env
```



忘记 `--from...` 的时候，不要查 kubernetes，而是使用 `kubectl create configmap --help` 快速查询文档。



**Secret**

secret 跟 comfigmap 类似，创建 secret：

```bash
kubectl create secret generic my-secret --from-literal='username=user' --from-literal='password=mypassword'
```

映射到 Pod 中：

```yaml
    env:
    - name: USER_NAME
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: username
```





**SecurityContext**

Create a pod called secbusybox with the image busybox which executes command sleep 3600 and makes sure any Containers in the Pod, all processes run with user ID 1000 and with group id 2000 and verify.

核心部分：

```yaml
spec:
  securityContext: # add security context
    runAsUser: 1000
    runAsGroup: 2000
```

如果要验证，则：

```bash
kubectl exec -it secbusybox -- sh
```



Create pod with an nginx image and configure the pod with capabilities NET_ADMIN and SYS_TIME verify the capabilities.

格式：

```yaml
securityContext:
      capabilities:
        drop:
          - all
        add: [“NET_ADMIN”]
```

按照题目，核心部分是：

```yaml
spec:
  containers:
  - image: nginx
    securityContext:
      capabilities:
        add: ["SYS_TIME", "NET_ADMIN"]
```





**ServiceAccount**

创建 ServiceAccount：

```
kubectl create sa admin
```



在 Pod 中使用此 ServiceAccount：

```bash
spec:
  serviceAccountName: admin
```





**资源限制**

Create a Pod nginx and specify a memory request and a memory limit of 100Mi and 200Mi respectively.

核心部分：

```yaml
    resources: 
      requests:
        memory: "100Mi"
      limits:
        memory: "200Mi"
```

Create a Pod nginx and specify a CPU request and a CPU limit of 0.5 and 1 respectively.

核心部分：

```yaml
    resources:
      requests:
        cpu: "0.5"
      limits:
        cpu: "1"
```



**可观测性**

Pod 有启动检查、就绪检查、健康检查，检测方式有 TCP、HTTP、命令等。

参考 

https://kubernetes.io/zh/docs/concepts/workloads/pods/pod-lifecycle/#container-probes

https://kubernetes.io/zh/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes

启动检查：startupProbe；

就绪检查：readinessProbe；

健康检查：livenessProbe；



两个常用参数：

- `initialDelaySeconds`：容器启动后要等待多少秒后存活和就绪探测器才被初始化，默认是 0 秒，最小值是 0。
- `periodSeconds`：执行探测的时间间隔（单位是秒）。默认是 10 秒。最小值是 1。

命令形式的检查：

```
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```

HTTP 形式的检查：

```yaml
  httpGet:
    httpHeaders:
      - name: Accept
        value: application/json
```

TCP：

```yaml
      tcpSocket:
        port: 8080
```





**网络**

创建带端口的 Pod：

```bash
kubectl run nginx --image=nginx --restart=Never --port=80
```

为一个 Pod 暴露端口：

```bash
kubectl expose po nginx --port=80 --type=NodePort
```

映射成不同的端口：

```bash
kubectl expose po nginx --port=80 --target-port=8080 --type=NodePort
```

