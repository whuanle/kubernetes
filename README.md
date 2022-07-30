# 文档说明

作者：痴者工良

作者博客地址：

[https://www.whuanle.cn](https://www.whuanle.cn)

[https://www.cnblogs.com/whuanle](https://www.cnblogs.com/whuanle)



本书文档地址：

[https://k8s.whuanle.cn](https://k8s.whuanle.cn)【适合国内访问】

[https://ek8s.whuanle.cn](https://ek8s.whuanle.cn) 【gitbook】



简介：

这是一本关于 Kubernetes 基础的书。

本书分为 五大章，每个大章一个目录，目录中都有一个导读，导读会大概介绍本章会讲解什么内容，需要掌握什么知识，每章中有多个小章，每个小章是一个单独的知识点。

本书覆盖了 Kubernetes 基础的大多数知识点，读者既可以了解具体的知识内容，也可以跟着做实验，因为每个部分都是经过笔者多次实践演练，得到结果验证，几乎每一章都有大量的实验。

第一章介绍了 Docker、Kubernetes 的一些基础知识；

第二章介绍了如何部署 Kubernetes 集群；

第三章介绍了 Pod 的大部分知识和实践实验，如何通过各种方式部署调度 Pod；

第四章介绍了  Kubernetes 的网络知识，解决如何解决应用的网络问题；

第五章介绍了存储卷的使用方法和分布式存储。



> 笔者注：云原生应用，不仅仅是用 Kubernetes 部署，还需要解决日志收集、集群/应用监控等一系列问题，使用合理的技术方案做好基础设施的架构。



* [1.基础知识](1.basic/README.md)
  * [导读](1.basic/README.md)
  * [1.1 说透 Docker：基础](1.basic/1.docker.md)
  * [1.2 说透 Docker： 虚拟化](1.basic/2.virtual.md)
  * [1.3 了解 Docker 网络](1.basic/3.docker_network.md)
  * [1.4 Docker 和 Pod](1.basic/4.pod_docker.md)
  * [1.5 K8S入门基础](1.basic/5.k8s.md)
* [2.部署和配置](2.deploy/README.md)
  * [导读](2.deploy/2.deploy.md)
  * [2.1 使用 Minikube 部署](2.deploy/1.minikube.md)
  * [2.2 使用 kubeadm 部署](2.deploy/2.kubeadm.md)
  * [2.3 CKAD认证中的部署教程](2.deploy/3.kubeadm_ckad.md)
  * [2.4 国内代理](2.deploy/4.kubeadm_proxy.md)
  * [2.5 Dashboard](2.deploy/5.dashboard.md)
* [3.Pod部署和调度](3.pod/README.md)
  * [导读](3.pod/README.md)
  * [3.1 Pod](3.pod/1.pod.md)
  * [3.2 Deployment部署](3.pod/2.deployment.md)
  * [3.3 副本集(ReplicaSet)](3.pod/3.replica.md)
  * [3.34 Pod 端口映射](3.pod/4.pod_network.md)
  * [3.5 Pod 升级、回滚](3.pod/5.update.md)
  * [3.6 Pod 缩放](3.pod/6.scale.md)
  * [3.7.Pod 标签](3.pod/7.lable.md)
  * [3.8 Pod 调度](3.pod/8.schedule.md)
  * [3.9 Jobs、CronJobs](3.pod/9.jobs_cronjobs.md)
* [4.Kubernetes 网络](4.network/README.md)
  * [导读](4.network/README.md)
  * [4.1 Kubernetes 网络](4.network/1.network.md)
  * [4.2 Endpoint](4.network/2.endpoint.md)
  * [4.3 ingress](4.network/3.ingress.md)
  * [4.4 服务发现](4.network/4.discovery.md)
* [5.volumes](5.volumes/README.md)
  * [导读](5.volumes/README.md)
  * [5.1 卷](5.volumes/1.volumes.md)
  * [5.2 secret 和 ConfigMap 卷](5.volumes/2.secret_configmap.md)
  * [5.3 NFS卷](5.volumes/3.nfts.md)
  * [5.4 持久化卷](5.volumes/4.pv_pvc.md)
* [Kubernetes 命令概览](k8s.md)
* [CKAD 认证帮助](ckad.md)
