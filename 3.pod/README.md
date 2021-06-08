# 第三章：Pod部署和调度



### 学习目标

**部署、配置、标签选择、调度、副本、控制器**

* 讨论部署配置细节
  * 通过 `kubectl create/apply` 创建 Deployment ，部署 Pod；
  * 通过 `kubectl edit` 修改 对象；
  * 如何查看对象信息，`kubectl get`、`kubectl describe`、`-o wide`、`-o yaml`;
  * 创建 Service，`kubectl expose`；
  * 设置副本集，`replicaset`；
* 向上和向下扩展部署。
  * 扩容 Pod，设置副本集，`kubectl scale`、`kubectl edit`；
  * 自动扩容 Pod，水平缩放，比例缩放，根据 CPU、内存缩放，`kubectl autoscale`；
  * DaemonSet
* 实现滚动更新和回滚
  * 更换镜像版本、更新 Pod，`kubectl set image`；
  * 设置滚动更新速度，`maxSurge`、`maxUnavailable`；
  * 暂停和恢复更新，`kubectl rollout pause` 、`kubectl rollout resume`；
  * 回滚旧版本，`kubectl rollout undo`；
* 使用标签选择各种对象以及调度
  * 了解 label、selector、nodeSelector；
  * 查询 label、选择 label、使用选择器调度 Pod；
  * 选择器运算符，等值选择、集合选择；
* 配置污点和容忍度
  * 亲和性和反亲和性
  * 配置污点和容忍度
  * 系统默认污点、master 的调度配置
  
* Job 和 CronJob
  * 了解 Job，完成数\(conpetions\)、工作队列\(parallelism\)、控制并行性、清理 Job。
  * CronJob 的时间表示

