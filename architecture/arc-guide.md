# 云原生架构落地指南



## 微服务化


对付复杂性的最好方法之一是将明确定义的功能分成更小的服务，并让每个服务独立迭代。这增加了应用程序的灵活性，允许根据需要更轻松地更改部分应用程序。每个微服务可以由单独的团队进行管理，使用适当的语言编写，并根据需要进行独立扩缩容。


## 将生产服务容器化

实现云原生架构的前置条件是服务容器化，没有容器化技术，云原生下的弹性、服务韧性、资源成本节省将不具备任何优势。

容器化云应用的基础是容器管理和编排服务。虽然存在各种各样的服务，但占据统治地位的显然是 Kubernetes，Kubernetes 建立了一个活跃的社区并获得了众多领先商业供应商的支持，已然成为行业中的容器编排标准。

Kubernetes 定义了被称为 Pod 的抽象。每个 Pod 通常只包含一个容器（例如图 4 中的 Pod A 和 B），但也可以包含多个容器（例如 Pod C）。每个 Kubernetes 服务运行一个包含一定数量节点的集群，每个节点通常是一个虚拟机。图 4 仅显示四个虚拟机，但一个真实的集群可能轻易就包含 100 个或更多虚拟机。当 Pod 部署到 Kubernetes 集群上时，服务会确定该 Pod 的容器应该在哪些虚拟机中运行。由于容器指定了其所需的资源，因此 Kubernetes 可以智能地选择为每个虚拟机分配哪些 Pod。

Pod 的部署信息会指明应该运行的 Pod 实例（副本）数量。Kubernetes 服务随后会创建该数量的 Pod 容器实例并分配给虚拟机。例如，在图 4 中，Pod A 和 Pod C 的 Deployment 都请求了三个副本。但是，Pod B 的 Deployment 请求了四个副本，因此，此示例集群包含容器 2 的四个运行实例。如图所示，包含多个容器的 Pod（例如 Pod C）的容器将始终被分配给相同节点。

Kubernetes 还提供其他服务，包括：

- 监控正在运行的 Pod，如果容器出现故障，服务将启动新实例。这可确保 Pod 的 Deployment 中请求的所有副本保持可用。
- 负载均衡流量，以智能方式将对每个 pod 发出的请求分散到容器的副本中。
- 自动零停机地发布新容器，新实例将逐步替换现有实例，直到新版本完全部署为止。
- 自动扩缩，集群根据需求自主添加或删除虚拟机。


<div  align="center">
	<img src="../assets/kubernetes.png" width = "550"  align=center />
</div>