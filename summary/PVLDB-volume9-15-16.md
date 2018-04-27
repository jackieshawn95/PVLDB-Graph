# [动态介数中心度]Fully Dynamic Betweenness Centrality Maintenance on Massive Networks
## 问题描述:
可扩展的动态方法计算介数中心度。
## 方法:
- 基于采样，重点维护每个采样对之间的单个最短路径。
- 主要数据结构：hypergraph sketch（草图），使用非均匀分布概率替换定点对。顶点介数中心度可以从hypergraph sketch中获得。
- 为了加速更新，两个辅助数据结构：

> two-ball index：基本保持最短路径树的子树，利用网络的小世界属性去实现更小的数据结构。  
> special-purpose reachability index：解决two-ball index不能处理不可达顶点对的问题。  

- TB索引可以有效检测顶点对之间的最短路径变化。边插入删除可以在几ms内处理。  

## 启示：
针对介数中心度这种需要大量顶点通信以及顶点存储的算法，可以使用某种采样和某种建立索引的方法来对算法进行优化。

# [图系统Benchmark]LDBC Graphalytics: A Benchmark for Large-Scale Graph Analysis on Parallel and Distributed Platforms
## 问题描述:
提出了LDBC Graphalytics，它是图处理平台的一个工业级Benchmark，由6个确定性算法，标准数据集，数据集生成器和参考输出。测试工具生成的metrics可以度量多种系统的扩展性。
## 本文贡献：
- Graphalytics是第一个覆盖压力测试和性能变化的图Benchmark。
- 幂律分布图生成基于Graph500，社交网络使用LDBC Datagen，可以生成较为真实的图数据集，可以指定聚类系数。
- 为目标系统提供了驱动程序和算法参考实现。目标系统有PGX、GraphMat、OpenG、Giraph、GraphX、Power graph，公有算法包括BFS，WCC，PR，SSSP，弱连通分量，LPA。  

<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/15-16-1.png"><br><br>
</div>  

- 测试通过纵向横向对比，强弱对比。  

> 强扩展性是指，保持数据集不变，增加资源。  
> 弱扩展性是指，保持资源不变，增加数据集。  
> 横向是指增加机器数量，纵向是指增减单个机器core的数量。使用加速比来衡量扩展性。

文章第4部分详细介绍了在各种对比测试中的结果，总结就是工业界（PGX、GraphMat、OpenG）的各种性能普遍优于社区版图处理平台（Giraph、GraphX、Power graph），包括单机多线程和集群测试。

## 启示：
GraphX性能较差的原因可能在于在集群情况下，处理某些算法需要大量的消息通信，可以针对消息的通信方面进行优化。

# [基于graphX的连通子图挖掘]GARUDA: A System for Large-Scale Mining of Statistically Significant Connected Subgraphs
## 问题描述:
基于graphX的大规模图上的子图挖掘系统。
## 本文贡献：
- 系统功能以及GUI描述。
- 架构说明。
- 演示场景。
- 比最先进的MSCS(Mining Significant Connected Subgraphs)算法加速8-10倍
- 可以查询 top-k子图，找到卡方值大于或等于用户定义阈值的子图，可以找到节点数目大于等于用户定义阈值的子图。  

<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/15-16-2.png"><br><br>
</div>

## 启示：
可以基于GraphX等系统做一些固定类型算法，比如找到用户定义中心度范围的顶点集，例如找到度中心度在[x,y]区间内的顶点集，可以选择使用哪些中心度。

