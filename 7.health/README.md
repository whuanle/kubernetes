# 8.集群维护与日志和故障排除



日常管理和维护：

节点维护命令  **cordon/uncordon**

kubectl cluster-info dump 查看集群日志

管理和插件整个集群的节点的日志。

管理容器标准输出和错误日志。包括了使用某些平台托管监控 pod 日志。



健康检查：

就绪探针，健康探针

容器管理工具

7.1pod的默认检查策略

7.2通过liveness对pod健康性检查

7.3使用readiness对pod健康性检查

7.4健康性检查在各种环境中的应用

Pod 存活探针

84 页

死锁避免，死锁预防，死锁检测，死锁解除，通过存活探针检测状态，也是进而被 kubernetes 杀死并重启。

探针的CPU时间，计入容器的CPU配额。



helm：

12.1Helm工具的架构和安装使用

12.2helm源管理

12.3搭建helm私有仓库

实战：用helm3部署EFK日志

实战：用helm3部署prometheus监控



