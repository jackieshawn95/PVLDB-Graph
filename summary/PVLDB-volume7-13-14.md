# [图计算系统对比]An Experimental Comparison of Pregel-like Graph Processing Systems
## 问题描述：
本文比较了类Pregel图处理系统的性能。
## 本文贡献：
- 在PageRank，单源最短路径，弱连通分量，分布式最小生成树上测试Giraph，GPS，Mizan，GraphLab的性能，发现Giraph、GraphLab全面性能比较好，GPS在内存利用率上较好（实验在128台亚马逊EC2上进行）。  
- GraphLab为异步模式，较大的通信开销导致扩展性较差。  
- Giraph更好的负载均衡可以减少最大内存使用量，需要邻接列表。  
- GPS利用数据的本地性减少消息开销。  
- Mizan增加系统和消息处理优化提高性能和扩展性。  

## 启示:
对类Pregel图计算平台来说，优化可以从同步异步模式以及优化通信机制入手。针对某些算法，或许可以采用某些丢失小部分精度的办法来降低时间以及空间复杂度。

# [编程模型]From "Think Like a Vertex" to "Think Like a Graph"
## 问题描述：
解决类Pregel编程模型会产生大量通信消息以及确保数据一致性所需大量开销，并且会忽略来自用户的分区信息问题。
## 本文贡献：
- 基于Giraph实现“以图为中心”的Giraph++，在1.18亿顶点，8亿条边的图上，连通分量算法比“以顶点为中心”的版本运行速度提高了63倍，大量减少了网络通信消息。
- 充分利用分区中的局部图结构，可以表达更加复杂和灵活的图算法，证明图遍历、随机游走、图聚合三类算法可以采用异步计算方式加快收敛速度。
- 图中心模型随着每次迭代，网络消息和执行时间都会减少，收敛所需迭代次数更少。

## 启发：
之前有一些算法在BSP模型上无法实现的原因就是，算法思想不是以顶点为中心的，或许采用这种编程模型可以实现。

# [类Pregel上的算法优化]Optimizing Graph Algorithms on Pregel-like Systems
## 问题描述：
在类Pregel平台上的算法虽然易于实现，但由于通信成本高，收敛速度慢导致性能不好。
## 方法：
基本思想是在输入图的一小部分上执行串行计算用来补充以顶点为中心的并行计算。
## 本文贡献：
以下是本文实现的图算法以及算法优化技术。
<div align="center">
  <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/13-14-1.png"><br><br>
</div>  
下表为使用的数据集：
<div align="center">
  <img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/13-14-2.png"><br><br>
</div> 

## 启发：
其中算法优化思想可以用在其他图计算平台上。