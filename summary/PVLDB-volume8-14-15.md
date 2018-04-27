# [图分片]A Scalable Distributed Graph Partitioner
## 问题描述：
图分片问题，比批处理METIS和流处理Fennel都快，分片与输入图的分布无关。
## 方法：
Sheep。  
通过分布式map-reduce将图转换为strictly smaller elimination tree。通过对树的分区，Sheep可以找到原始图的上界分区。相比较其他的分区器，Sheep可以扩展到更大的图，并且时间更少。
## 本文贡献:
- 不牺牲分区质量的基础上，在4分钟内有效分配有数十亿条边的图，也可以与流式分区器相提并论。
- 在分区质量，树分解理论，中心分析之间建立关系。  

## 启示：
本文对于处理流式分区与spark处理流式数据策略类似，都是采用微批式模拟流数据。

# [失效恢复]Fast Failure Recovery in Distributed Graph Processing Systems
## 问题描述：
解决因为增加处理节点而导致的故障问题。
## 方法：
关键方法是将丢失的一部分图进行重新划分到剩余节点上，其实就是将恢复任务分配给多个节点，同时执行恢复处理，利用对恢复过程的计算量和通信成本敏感的分区机制来增强现有的基于检查点和日志的恢复方案。在40个节点集群上的性能优于检查点恢复，最高可达30倍。
## 启示：
可以比较现有的恢复方案在不同分布图与不同图计算场景下的性能，总结出在什么情况下用哪种恢复方案。

# [BAP异步模型]Giraph unchaind, Barrierless Asynchronous Parallel Execution in Pregel-like Graph Processing Systems
## 问题描述：
解决BSP模型虽然可以使算法易于实现，但被频繁的全局同步和过时消息影响性能的问题。并且现有异步模型扩展性较差，频繁维护全局的barriers，不能较好的支持有多个计算阶段的算法。
## 本文贡献：
- 提出了BAP( Barrierless Asynchronous Parallel)模型，可以解决上述问题。
- 在开源Giraph上实现了基于BAP模型的Giraph UI。
- 在64个EC2上进行试验，比同步Giraph，异步Giraph，同步GraphLab系统快5倍，比异步GraphLab快86倍，保留同步算法的高效编程特性。  

## 方法：
- 使用本地barriers和逻辑超步确保每个计算阶段仅使用一个全局barrie完成，降低超步开销。
- 常规BSP：如果所有顶点都处于非活跃状态并且没有未处理消息，则终止。
<div align="center">
  <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-1.png"><br><br>
</div>
如图(a)，普通方法使用全局Barrier导致W1和W3长期被阻塞，当W1、W3执行作业时，W2又长期被阻塞，导致集群利用率低。如图(b)，使用轻量级的barrier，允许worker在接收到消息时解除block状态，消息分批到达可以让worker总有任务可以执行。确保算法使用最少的全局超步，减少通信和同步开销。
## 启示：
该模型结合了同步与异步模型的特点，并且保留了同步编程模型的易理解特性。但这种模型感觉对于某些单源算法有较大提升，比如SSSP，对于其他类型的图算法，哪种模型比较优秀有待进一步测试。

# [图生成]GraphGen: Exploring Interesting Graphs in Relational Data
## 问题描述：
图数据并不是存储主流选择，一般都是关系型数据库，但图计算场景却非常重要。解决了在关系数据库上直接执行图抽取任务，可视化浏览图。

# [图键]Keys for Graphs
## 问题描述：
图Key唯一标识图中顶点代表的实体。
## 本文贡献：
- 提出了一种根据图pattern递归定义的key，用来标识子图同构。Key扩展了关系和XML，可以应用在对象识别，知识融合，社交网络。
- 研究了实体识别问题，给定一个图G和一个key集合，找到在图G中被Key定义的所有实体对。提供了两种并行可扩展的实体匹配算法，分别使用MapReduce和VC异步模型。

## 启示：
可以利用建立Key的方式进行模式匹配，子图匹配。

# [图计算系统对比]Large-Scale Distributed Graph Computing Systems: An Experimental Evaluation
## 问题描述：
研究Giraph，GraphLab/ PowerGraph , GPS , Pregel+ , GraphChi性能对比。
## 本文方法：
- 各种系统在不同算法类别的性能。always-activealgorithms,graph-traversal,multi-phase algorithms,graph mutation,
- 各种系统在不同特征的图上的性能。
- 分布式系统与单机系统相比（与GraphChi对比）  

## 本文贡献：
- 没有研究GraphX是因为其基于Spark，拿运行一个PageRank来说，第一步需要提取图并存储，第二步进行计算，第一步需要转储到HDFS，第二部要从HDFS上重新加载数据，这样相比较其他的分布式图计算系统来说，仅从时间考虑GraphX是不占优势的。
- Pregel+和GPS比Giraph和GraphLab具有更好的整体性能，Pregel得益于消息combing(可以减少通信开销)，镜像组合，请求相应技术，其内部使用C++实现可以减少使用内存，比Java实现的系统快2-3倍。GPS主要是得益于LALP技术，GraphLab使用顶点切割分片进行负载均衡，Giraph 没有采用特定技术处理图倾斜，主要靠combiner减少消息传递。
<div align="center">
  <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-2.png"><br><br>
</div>

- <div align="center">
  <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-3.png"><br><br>
</div>
 <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-4.png"><br><br>
</div>
 <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-5.png"><br><br>
</div>
 <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-6.png"><br><br>
</div>
 <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-7.png"><br><br>
</div>
 <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-8.png"><br><br>
</div>
 <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-9.png"><br><br>
</div>
 <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-10.png"><br><br>
</div>
 <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/14-15-11.png"><br><br>
</div>

















