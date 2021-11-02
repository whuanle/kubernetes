# 5.volumes

存储卷、向容器写入内容、访问数据、etcd





了解卷；

学会创建 emtpydir、Hostpath、ntfs 卷。

理解并创建持久卷(PV)。

配置持久卷声明(PVC)。

管理卷访问方式。

部署一个能够访问持久存储的应用程序。

讨论存储的动态配置。

配置 Secret 和 ConfigMaps

环境变量和数据传递

5.1使用secret管理密码

5.2以卷的方式引用密码，传递配置文件

5.3以变量的方式引用密码

5.4使用configmap管理密码

### Downward API介绍

 作用:让Pod里的容器能够直接获取到这个Pod API**对象本身的信息**

etcd，etcd 快照与恢复

