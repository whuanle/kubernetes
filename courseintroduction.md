# courseintroduction

考试课程包括这些一般领域及其在考试中的权重：

| 核心概念 - 13％ | 配置 - 18％     | Multi-Container Pods  - 10％ |
| ---------- | ------------ | --------------------------- |
| 可观察性 - 18％ | Pod 设计 - 20％ | 服务与网络 - 13％                 |
| 持续状态 - 8％  |              |                             |

**详细内容**

**核心概念 - 13％**

• 了解Kubernetes API原语

• 创建和配置基本窗格

**配置 - 18％**

• 了解ConfigMaps

• 了解SecurityContexts

• 定义应用程序的资源需求

• 创造和消费秘密

• 了解ServiceAccounts

**Multi-Container Pods - 10％**

• 了解Multi-Container Pod设计模式（例如，大使，适配器，边车）

**可观察性 - 18％**

• 了解LivenessProbes和ReadinessProbes

• 了解容器日志记录

• 了解如何监控Kubernetes中的应用程序

• 了解Kubernetes中的调试

**Pod设计 - 20％**

• 了解如何使用标签，选择器和注释

• 了解部署以及如何执行滚动更新

• 了解部署以及如何执行回滚

• 了解Jobs和CronJobs

**服务与网络 - 13％**

• 了解服务

• 展示对NetworkPolicies的基本了解

**持续状态 - 8％**

## 范围

**集群架构，安装和配置**

* 管理基于角色的访问控制(RBAC)
* 使用Kubeadm安装基本集群
* 管理高可用的Kubernetes集群
* 提供底层基础设施部署Kubernetes集群
* 使用Kubeadm在Kubernetes集群上进行版本升级
* 实现etcd备份和恢复
* 了解部署以及如何执行滚动更新和回滚
* 使用ConfigMaps和Secrets配置应用程序
* 了解如何扩展应用程序
* 理解用于创建健壮的、自修复的应用程序部署的原语
* 了解资源限制如何影响Pod调度
* 了解清单管理和常用模板工具

**服务网络**

* 了解集群节点上主机的组网配置
* 理解Pods之间的连接
* 了解ClusterIP、NodePort、LoadBalancer服务类型和端点
* 了解如何使用Ingress控制器和Ingress资源
* 了解CoreDNS的配置和使用
* 选择一个合适的容器网络接口插件

**存储**

* 理解存储类、持久卷
* 了解卷模式、访问模式和卷回收策略
* 理解持久卷声明原语
* 了解如何使用持久存储配置应用程序

**故障排除**

* 评估集群和节点日志记录
* 了解如何监视应用程序
* 管理容器标准输出和标准错误日志
* 排除应用程序失败
* 排除集群组件故障
* 排除网络
