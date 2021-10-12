## 硬件抽象级别虚拟化虚拟化技术

### 理论基础

计算机组成原理：
随着计算机技术的发展，计算机的某些功能即可以由硬件实现，也可以由软件来实现。
			--  软件和硬件在功能意义上是等效的。

一个功能使用硬件来实现还是使用软件来实现？

硬件实现：速度快、成本高；灵活性差、占用内存少。

软件实现：速度低、复制费用低；灵活性好、占用内存多。

虚拟化技术是将原本 硬件实现的功能，使用软件来实现。





### 不同层次的虚拟化

![virstual](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\virstual.jpg)

模拟和仿真（计算机体系结构中的）





### 虚拟化干了什么

学术定义：虚拟化（技术）或虚拟技术是一种资源管理技术，将计算机的各种实体资源（CPU、内存、磁盘空间、网络适配器等），予以抽象、转换后呈现出来并可供分割、组合为一个或多个计算机配置环境。



### 传统虚拟化部署方式

硬件抽象级别虚拟化。

特点是 虚拟化程度高

![traditional_kvm ](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\traditional_kvm .jpg)

1，虚拟机之间通过虚拟化技术隔离互不影响
2，物理机上可部署多台虚拟机，提升资源利用率
3，应用资源分配、扩容通过虚拟管理器直接可配置
4，支持快照、虚拟机克隆多种技术，快速部署、容灾减灾

传统虚拟化部署方式-缺点

1， 资源占用高	需要额外的操作系统镜像，需要占用GB级别的内存以及数十GB存储空间。
 2，启动速度慢	虚拟机启动需要先启动虚拟机内操作系统，然后才能启动应用。
 3，性能影响高应用 => 虚拟机操作系统=> 物理机操作系统=> 硬件资源





## Docker



### Docker

Docker是一个开放源代码软件项目，自动化进行应用程序容器化部署，借此在Linux操作系统上，提供一个额外的软件抽象层，以及操作系统层虚拟化的自动管理机制。  
-From wiki

![docker_logo](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_logo.jpg)



### 虚拟机和 Docker 的差异

| ** **    | **虚拟机**                       | **Docker 容器**                              |
| -------- | -------------------------------- | -------------------------------------------- |
| 隔离程度 | 硬件级进程隔离                   | 操作系统级进程隔离                           |
| 系统     | 每个虚拟机都有一个单独的操作系统 | 每个容器可以共享操作系统（共享操作系统内核） |
| 启动时间 | 需要几分钟                       | 几秒                                         |
| 体积大小 | 虚拟机镜像需要几GB               | 容器是轻量级的（KB/MB）                      |
| 启动镜像 | 虚拟机镜像比较难找到             | 预建的 docker 容器很容易获得                 |
| 迁移     | 虚拟机可以轻松迁移到新主机       | 容器被销毁并重新创建而不是移动               |
| 创建速度 | 创建 VM 需要相对较长的时间       | 可以在几秒钟内创建容器                       |
| 资源使用 | 更多资源使用                     | 更少的资源使用                               |

![virstual_location](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\virstual_location.jpg)

### 什么是容器化应用

containerized applications 指容器化的应用，我们常常说使用镜像打包应用程序，使用 Docker 发布、部署应用程序，那么当你的应用成功在 Docker 上运行时，称这个应用是 containerized applications。





Linux 内核的用途是什么？

内存管理：追踪记录有多少内存存储了什么以及存储在哪里

进程管理：确定哪些进程可以使用中央处理器（CPU）、何时使		    用以及持续多长时间

设备驱动程序：充当硬件与进程之间的调解程序/解释程序

系统调用和安全防护：从流程接受服务请求

![linux_kernel](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\linux_kernel.jpg)

![linux_docker](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\linux_docker.jpg)



### 应用怎么打包

编写 Dockerfile 文件	       -- 定义构建镜像的流程

选择一个基础镜像（操作系统）           -- 操作系统

安装应用的需要的环境		 -- 环境

复制程序文件			 -- 应用

开始镜像的制作			-- 生成镜像

Docker 镜像组成

![1633930417(1)](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\1633930417(1).jpg)

通常Docker镜像中只有一个精简的Shell，甚至没有Shell。镜像中还不包含内核——容器都是共享所在Docker主机的内核。所以有时会说容器仅包含必要的操作系统（通常只有操作系统文件和文件系统对象）。

![docker_dotnet](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_dotnet.jpg)



### Docker 工作流程

![docker_work1](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_work1.jpg)

![docker_work2](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_work2.jpg)

## Docker 结构



### Docker 服务与客户端

```
Client: Docker Engine - Community
 Version:           20.10.7
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        f0df350
 Built:             Wed Jun  2 11:58:10 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.7
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       b0f5bc3
  Built:            Wed Jun  2 11:56:35 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.6
  GitCommit:        d71fcd7d8303cbf684402823e425e9dd2e99285d
 runc:
  Version:          1.0.0-rc95
  GitCommit:        b9ee9c6314599f1b4a7f497e1f1f856fe433d3b7
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

> 执行 docker  version 查看版本号



### Docker客户端

Docker客户(DockerClient） 是一个命令行程序，使用 Go 语言编写，我们也可以使用 C#、Java 等语言写一个。

客户端通讯

Docker 客户端和服务端使用 REST API、UNIX 套接字或网络接口进行通信。
Docker 客户端 ( docker) 是许多 Docker 用户与 Docker 交互的主要方式。当您使用诸如 之类的命令时docker run，客户端会将这些命令发送到dockerd，从而执行它们。

什么是域套接字

在 Linux 中，有很多进程，为了让多个进程能够进行通讯，出现和很多方法，其中一种是套接字(socket)。一般的 socket 都是基于 TCP/IP 的，称为网络套接字，可以实现跨主机进程通讯。在 Linux 中有一种套接字，名为域套接字，只能用于在同一计算机中的进程间通讯，但是其效率高于网络套接字。域套接字使用一个 .sock 文件进行通讯。

| 运行时     | 域套接字                        |
| ---------- | ------------------------------- |
| Docker     | /var/run/dockershim.sock        |
| containerd | /run/containerd/containerd.sock |
| CRI-O      | /var/run/crio/crio.sock         |

![docker_client_server](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_client_server.jpg)

### Docker引擎

Docker 引擎由 Docker守护进程（Docker daemon）、containerd以及runc组成。

Docker daemon

Docker 守护进程 ( dockerd) 侦听 Docker API 请求并管理 Docker 对象，例如图像、容器、网络和卷。

![docker_daemon](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_daemon.jpg)

### Docker旧版本引擎

Docker首次发布时，Docker引擎由两个核心组件构成：LXC和Docker daemon。

LXC (Linux Container)是 Linux 提供的一种内核虚拟化技术，可以提供轻量级的虚拟化，以便隔离进程和资源。

LXC提供了对诸如命名空间（Namespace）和控制组（CGroup）等基础工具的操作能力，它们是基于Linux内核的容器虚拟化技术。

Docker 抛弃 LXC

首先，LXC 是基于 Linux 的。这对于一个立志于跨平台的项目来说是个问题。其次，如此核心的组件依赖于外部工具，这会给项目带来巨大风险，甚至影响其发展。

![docker_lxc](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_lxc.jpg)

### Docker引擎的架构

containerd

containerd 位于 daemon 和 runc 所在的 OCI 层之间。
它的主要任务是容器的生命周期管理：
——start | stop | pause | rm....

shim

容器运行时 shim 是一种位于容器管理器和容器运行时（runc、crun）之间的软件，
用于解决这些对应物的集成问题。
它的作用非常单一，那就是实现 CRI 规定的每个接口，然后把具体的 CRI 请求“翻译”成对后端容器项目的请求或者操作。

runc

runc 实质上是一个轻量级的、针对 Libcontainer 进行了包装的命令行交互工具（Libcontainer取代了早期Docker架构中的LXC）。
runc 生来只有一个作用——创建容器

OCI定义了容器运行时标准，runC是Docker按照开放容器格式标准制定的一种具体实现。

![docker_struct](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_struct.jpg)

![docker_struct2](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_struct2.jpg)



## Docker核心技术

### Linux-Namespace

Namespace 是 Linux 内核提供的一种资源隔离技术，目前实现了以下7种资源隔离，Docker 也基本在这七种资源上对容器环境进行隔离。

![docker_namespace](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\linux_namespace.jpg)



### Linux-进程树

Linux的进程有父进程、子进程之分；将进程间的关系绘制出来，就会发现是一个“树”的结构。

![linuxps](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\linuxps.jpg)

![linux_process](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\linux_process.jpg)





#### Docker-Namespace 进程隔离

![docker_run1](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_run1.jpg)

![docker_run2](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_run2.jpg)

![docker_run3](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_run3.jpg)





### Linux-Cgroups资源隔离

Cgroups是control groups的缩写，是Linux内核提供的一种可以限制、记录、隔离进程组（process groups）所使用的物理资源（如：cpu,memory,IO等等）的机制。

![docker_groups](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_groups.jpg)

Linux-Cgroups功能

Resource limitation: 限制资源使用，比如内存使用上限以及文件系统的缓存限制。
Prioritization: 优先级控制，比如：CPU利用和磁盘IO吞吐。
Accounting: 一些审计或一些统计，主要目的是为了计费。
Control: 挂起进程，恢复执行进程。

Docker 限制容器能够使用的资源量参数：
-m 4G --memory-swap 0 --cpu-period=1000000 --cpu-quota=8000000 







## Go 简单实现 进程隔离



## Docker 网络

Docker 网络

Docker 网络架构源自一种叫作容器网络模型（CNM）的方案，该方案是开源的并且支持插接式连接。Libnetwork 是 Docker 对 CNM 的一种实现，提供了 Docker 核心网络架构的全部功能。不同的驱动可以通过插拔的方式接入 Libnetwork 来提供定制化的网络拓扑。



### Docker 网络类型

•基础理论
•单机桥接网络
•多机覆盖网络
•接入现有网络
•服务发现
•Ingress网络





### Docker 网络-基础理论

Docker网络架构由3个主要部分构成：CNM、Libnetwork和驱动

CNM是设计标准：在 CNM中，规定了Docker网络架构的基础组成要素；

Libnetwork：CNM 规范的实现，Libnetwork 通过Go语言编写；

驱动：通过实现特定网络拓扑的方式来拓展该模型的能力；

![docker_network_struct](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_network_struct.jpg)



### Docker 网络-基础理论（CNM）

CNM定义了3个基本要素：沙盒（Sandbox）、终端（Endpoint）和网络（Network）。

沙盒是一个独立的网络栈。其中包括以太网接口、端口、路由表以及DNS配置。

终端就是虚拟网络接口，终端主要职责是负责创建连接。在CNM中，终端负责将沙盒连接到网络。

网络是802.1d网桥（类似大家熟知的交换机）的软件实现。

![docker_cnm](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_cnm.jpg)

### 四类网络模式

| Docker网络模式 | 配置        | 说明                                                         |
| -------------- | ----------- | ------------------------------------------------------------ |
| host模式       | –net=host   | 容器和宿主机共享Network namespace。                          |
| container模式  | –net={id}   | 容器和另外一个容器共享Network namespace。 kubernetes中的pod就是多个容器共享一个Network namespace。 |
| none模式       | –net=none   | 容器有独立的Network namespace，但并没有对其进行任何网络设置，如分配veth pair 和网桥连接，配置IP等。 |
| bridge模式     | –net=bridge | （默认为该模式）                                             |



### bridge 模式

bridge 模式称为网桥模式，首先会在主机上创建一个名为docker0的虚拟网桥，这个虚拟网络处于七层网络模型的数据链路层。

一般情况下，网桥默认 IP 范围是 172.17.x.x 。
使用 bridge 模式新创建的容器，其内部都有一个虚拟网卡，网卡地址是 172.17.x.x。容器之间可以通过 172.17.x.x 相互访问。

使用了 bride 创建的容器，其网络与主机以及其他容器隔离，以太网接口、端口、路由表以及DNS配置 都是独立的，即 CNM 中的 沙盒部分。

而虚拟网卡属于 CNM 中的终端，即虚拟网络接口。

如右图。容器 1 可以通过 172.17.0.3 访问容器 2；
容器可以通过 172.17.0.1 访问主机上的应用；
主机可以通过 172.17.0.x 访问任意一个容器。

每个容器都好像是一个独立的主机 -- 这便是 bridge（网桥）的作用。

![docker_bridge](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_bridge.jpg)

![docker_bridge2](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_bridge2.jpg)



### none 模式

这种网络模式下容器只有lo回环网络，没有其他网卡，这种类型的网络没有办法联网，封闭的网络能很好的保证容器的安全性。



![docker_none](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_none.jpg)

### host 模式

可以看作是 bridge 模式，并且在 bridge模式的网络隔离上，
与主机共享端口范围，即端口映射。

除了映射的端口可能会生产冲突外，容器的其余部分依然是隔离的。

![docker_host](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_host.jpg)

### container 模式

container 模式可以让多个容器之间相互通讯，即容器之间共享网络。

![docker_network_container](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_network_container.jpg)

![docker_network_container2](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_network_container2.jpg)

![docker_network_container3](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\docker_network_container3.jpg)

## Docker 存储

### 什么是文件系统

文件系统是操作系统用于在存储设备上组织文件的方法。
操作系统中负责管理和存储文件信息的软件机构称为文件管理系统，简称文件系统。
文件系统由三部分组成：文件系统的接口，对对象操纵和管理的软件集合，对象及属性。![file_system](E:\临时\新建文件夹\kubernetes_ckad\1.basic\.images\file_system.jpg)

### Linux- Union FS(联合文件系统)

Union FS：将不同文件夹中的层联合（Union）到了同一个文件夹中，这些文件夹在 AUFS 中称作分支，整个『联合』的过程被称为联合挂载（Union Mount）

Docker 镜像分层(联合文件系统)

镜像在打包时，文件一层层叠加；容器启动时在这些文件层中操作。
每一层都是固定不变的，上一层对下一层的操作都是逻辑操作，只是标记为 “删除”等实际上物理文件没有任何变化。

