# 4.Service和Ingress

## 第四章：Service和Ingress

## 1.学习目标

**服务与网络 - 13％**

• 了解服务

• 展示对NetworkPolicies的基本了解

**Service 部分**

解释Kubernetes服务。

应用程序公开。

讨论可用的服务类型。

启动本地代理。

使用集群DNS

**Ingress** 部分

讨论入口控制器和服务之间的区别。

了解nginx和GCE输入控制器。

部署Ingress Controller。

配置入接口规则。

对于 Ingress 的内容，需要了解那些知识和概念，对于实践方面，需要学会配置规则即可。

**高可用(HIGH AVAILABILITY)**

讨论Kubernetes中的高可用性。

讨论并置数据库和非并置数据库。

学习Kubernetes中实现高可用性的步骤。



•基础理论
•单机桥接网络
•多机覆盖网络
•接入现有网络
•服务发现
•Ingress网络



coredns

### **10.网络策略及资源限制**

10.1配置calico网络实现跨节点docker容器通信

10.2网络策略

限制同一命名空间里的pod的访问

允许指定命名空间里的pod访问

允许指定命名空间里特定的pod访问

egress策略的使用

默认策略
