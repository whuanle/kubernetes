# Ingress

Ingress 是对集群中服务的外部访问进行管理的 API 对象，它允许从 Kubernetes 集群外部访问 Kubernetes 服务，典型的访问方式是 HTTP。Ingress 可以提供负载均衡、SSL 终结和基于名称的虚拟服务器，这几个概念听起来较为模糊，后面会详细说明。

[Ingress](https://kubernetes.io/zh/docs/concepts/services-networking/ingress/)是一种 API 对象，其中定义了一些规则使得集群中的 服务可以从集群外访问。

[Ingress](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.21/#ingress-v1beta1-networking-k8s-io) 公开了从集群外部到集群内[服务](https://kubernetes.io/zh/docs/concepts/services-networking/service/)的 HTTP 和 HTTPS 路由。 流量路由由 Ingress 资源上定义的规则控制。

可以将 Ingress 配置为服务提供外部可访问的 URL、负载均衡流量、终止 SSL/TLS，以及提供基于名称的虚拟主机等能力。 [Ingress 控制器](https://kubernetes.io/zh/docs/concepts/services-networking/ingress-controllers) 通常负责通过负载均衡器来实现 Ingress，尽管它也可以配置边缘路由器或其他前端来帮助处理流量。



## 为何使用 Ingress

在前面，我们已经学习到了 Service，通过 Service 我们可以暴露一个端口到外网中，通过这个端口可以访问多副本(ReplicaSet)的 Pod。

其中，有两种方法可以暴露 Service：

- 使用 [Service.Type=LoadBalancer](https://kubernetes.io/zh/docs/concepts/services-networking/service/#loadbalancer)
- 使用 [Service.Type=NodePort](https://kubernetes.io/zh/docs/concepts/services-networking/service/#nodeport)



如果你有一个 `example.com` 域名，你部署了多个 Web 服务，其中有两个子模块分别为课程(course)、考试(exam) 两个微服务，这些模块构成了一个培训网站。此时我们希望访问 `example.com/api/course` 能够访问课程学习模块，访问 `example.com/api/exam` 能够访问考试模块。

在传统单体 Web 中，通过路由(route)能够实现不同后缀访问不同模块，但是现在是微服务，不是单体。

使用 Service 可以为每个服务公开一个端口，那么 N 个服务，就需要创建 N 个 Service。Service 虽然能够公开端口到外部网络中，但是无法将这些服务合并到一个 `example.com/{服务}` 中访问，Service 需要通过不同的端口访问。

使用 Ingress ，可以轻松设置路由规则，而且无需创建一堆 LoadBalancers/Nodes 公开每个服务，并且 Ingress 具有很多功能。

Ingress 也需要 Service 。

每次您要将服务公开给外界时，都必须创建一个新的LoadBalancer并获取一个IP地址。

Ingress 不会公开任意端口或协议。 将 HTTP 和 HTTPS 以外的服务公开到 Internet 时，通常使用 [Service.Type=NodePort](https://kubernetes.io/zh/docs/concepts/services-networking/service/#nodeport) 或 [Service.Type=LoadBalancer](https://kubernetes.io/zh/docs/concepts/services-networking/service/#loadbalancer) 类型的服务。

## 术语

为了表达更加清晰，Kubernetes 文档中定义了以下术语：

- 节点（Node）: Kubernetes 集群中其中一台工作机器，是集群的一部分。
- 集群（Cluster）: 一组运行由 Kubernetes 管理的容器化应用程序的节点。 在此示例和在大多数常见的 Kubernetes 部署环境中，集群中的节点都不在公共网络中。
- 边缘路由器（Edge router）: 在集群中强制执行防火墙策略的路由器（router）。 可以是由云提供商管理的网关，也可以是物理硬件。
- 集群网络（Cluster network）: 一组逻辑的或物理的连接，根据 Kubernetes [网络模型](https://kubernetes.io/zh/docs/concepts/cluster-administration/networking/) 在集群内实现通信。
- 服务（Service）：Kubernetes [Service](https://kubernetes.io/zh/docs/concepts/services-networking/service/)使用 [标签](https://kubernetes.io/zh/docs/concepts/overview/working-with-objects/labels/)选择算符（selectors）标识的一组 Pod。 除非另有说明，否则假定服务只具有在集群网络中可路由的虚拟 IP。



### Ingress 结构

Ingress 由 Ingress Controller、Ingress API 两部分组成，为了让 Ingress 资源工作，集群必须有一个正在运行的 Ingress 控制器。 [Ingress 控制器](https://kubernetes.io/zh/docs/concepts/services-networking/ingress-controllers/) 负责满足 Ingress 中所设置的规则。

Ingress 控制器不是随集群自动启动的。 基于此页面，你可选择最适合你的集群的 ingress 控制器实现，目前支持和维护 [AWS](https://github.com/kubernetes-sigs/aws-load-balancer-controller#readme)， [GCE](https://git.k8s.io/ingress-gce/README.md) 和 [nginx](https://git.k8s.io/ingress-nginx/README.md#readme) Ingress 控制器。

只要求掌握 Ingress 控制器。

其它控制器由经常提及到的 Istio，详细可以参考官方完整名单

[https://kubernetes.io/zh/docs/concepts/services-networking/ingress-controllers/#其他控制器](https://kubernetes.io/zh/docs/concepts/services-networking/ingress-controllers/#其他控制器)

https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/

### Ingress 部署过程、环境准备

1，部署一个 nginx-ingress-controller-pod 

https://kubernetes.github.io/ingress-nginx/deploy/

2，创建一个或多个 Service

3，创建ingress，配置路由规则



### Ingress 类型

在深入 Ingress 之前，我们来了解一下 Ingress 的类型/架构，也称为调度方式。



#### 单个 Service

将所有流量都发送到同一 Service 的简单 Ingress ，通过指定无规则的 *默认后端* 来对 Ingress 进行此操作。

DefaultBackend

没有 `rules` 的 Ingress 将所有流量发送到同一个默认后端。 `defaultBackend` 通常是 [Ingress 控制器](https://kubernetes.io/zh/docs/concepts/services-networking/ingress-controllers) 的配置选项，而非在 Ingress 资源中指定。

如果 `hosts` 或 `paths` 都没有与 Ingress 对象中的 HTTP 请求匹配，则流量将路由到默认后端。

![单个Service](H:\文章\K8S基础教程与CKAD认证\8.Ingress和高可用\.images\单个Service.png)



#### 简单扇出

一个扇出（fanout）配置根据请求的 HTTP URI 将来自同一 IP 地址的流量路由到多个 Service。 Ingress 允许你将负载均衡器的数量降至最低。

称为 URL 映射。

![简单扇出](H:\文章\K8S基础教程与CKAD认证\8.Ingress和高可用\.images\简单扇出.png)



#### 基于名称的虚拟托管

基于名称的虚拟主机支持将针对多个主机名的 HTTP 流量路由到同一 IP 地址上。

称为虚拟主机

![基于名称的虚拟托管](H:\文章\K8S基础教程与CKAD认证\8.Ingress和高可用\.images\基于名称的虚拟托管.png)

#### TLS

你可以通过设定包含 TLS 私钥和证书的[Secret](https://kubernetes.io/zh/docs/concepts/configuration/secret/) 来保护 Ingress。 Ingress 只支持单个 TLS 端口 443，并假定 TLS 连接终止于 Ingress 节点 （与 Service 及其 Pod 之间的流量都以明文传输）。 如果 Ingress 中的 TLS 配置部分指定了不同的主机，那么它们将根据通过 SNI TLS 扩展指定的主机名 （如果 Ingress 控制器支持 SNI）在同一端口上进行复用。 TLS Secret 必须包含名为 `tls.crt` 和 `tls.key` 的键名。 这些数据包含用于 TLS 的证书和私钥。例如：



#### 负载均衡

Ingress 控制器启动引导时使用一些适用于所有 Ingress 的负载均衡策略设置， 例如负载均衡算法、后端权重方案和其他等。 更高级的负载均衡概念（例如持久会话、动态权重）尚未通过 Ingress 公开。 你可以通过用于服务的负载均衡器来获取这些功能。

值得注意的是，尽管健康检查不是通过 Ingress 直接暴露的，在 Kubernetes 中存在并行的概念，比如 [就绪检查](https://kubernetes.io/zh/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)， 允许你实现相同的目的。 请检查特定控制器的说明文档（ [nginx](https://git.k8s.io/ingress-nginx/README.md)， [GCE](https://git.k8s.io/ingress-gce/README.md#health-checks)） 以了解它们是怎样处理健康检查的。

