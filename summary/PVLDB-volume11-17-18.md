# [图计算应用研究]The Ubiquity of Large Graphs and Surprising Challenges of Graph Processing
## 问题描述：
图处理问题在实际中的应用情况。  
旨在了解：①用户使用的图类型。②用户使用的图计算应用。③用户使用的图计算平台。④图处理面临的挑战。
## 本文贡献：
多样性：与产品，订单有关的商业数据较为典型。   
大规模图的普遍性：超过10亿条边的图很常见，不止存在于少数大公司(facebook/google)。涵盖了社交、科学。RDF、产品、数字数据。  

<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-1.png"><br><br>
</div>   

面临的问题：①扩展性挑战，有效处理超大图的能力是平台的瓶颈。②图可视化问题。③图查询语言问题。   
关系型数据库依旧是管理和处理图的主要工具。  
在图上的机器学习，顶点聚类、链接预测、发现重要顶点。  

## 方法：
调查了89位用户，并审查了22款软件产品的用户电子邮件和代码库。

## 结论：
- 图数据类型以商品交易订单为主。
- 查询语言与API是主要挑战。

以下图表为调查统计结果：
P代表从业人员、R代表研究人员
<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-2.png"><br><br>
</div>  
<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-3.png"><br><br>
</div>  
<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-4.png"><br><br>
</div>  
<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-5.png"><br><br>
</div>  
<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-6.png"><br><br>
</div>  
<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-7.png"><br><br>
</div>  
<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-8.png"><br><br>
</div>  
<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-9.png"><br><br>
</div>  
<img src="https://github.com/jackieshawn95/PVLDB-Graph/blob/master/pics/17-18-10.png"><br><br>
</div>  
















## 启示:
可以根据人们经常使用的图类型以及图计算需求，建立某种针对图的查询语言。