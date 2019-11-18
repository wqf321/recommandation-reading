motivation:大图中节点的低维嵌入已被证明在各种预测任务中非常有用，从内容推荐到识别蛋白质函数。但是，大多数现有方法都要求在训练嵌入过程中存在图中的所有节点；这些先前的方法本质上是转导性的，不能自然地推广到看不见的节点。为此提出一个Graphsage方法，这是一个通用的归纳框架，它利用节点特征信息（例如，文本属性）为以前看不见的数据有效地生成节点嵌入。这种归纳能力对于高吞吐量的生产机器学习系统至关重要。  
challenges:1.与转导相比，归纳式节点嵌入问题尤其困难，因为泛化到看不见的节点需要将新观察到的子图“对齐”到算法已经在其上优化的节点嵌入。归纳式必须识别节点邻域的结构属性，这些结构属性既可以显示节点在图中的本地角色，也可以显示其全局位置。大多数现有的生成节点嵌入的方法本质上都是转导的。 这些方法中的大多数使用基于矩阵分解的目标直接优化每个节点的嵌入，并且不会自然地泛化为看不见的数据，因为它们在单个固定图上的节点上进行预测，但是这些修改在计算上往往很昂贵，因此在做出新的预测之前还需要进行另外几轮的梯度下降。  
method:我们提出了一个通用的框架，称为GraphSAGE,用于归纳节点嵌入。与基于矩阵分解的嵌入方法不同，我们利用节点功能（例如，文本属性，节点配置文件信息，节点度）来学习可泛化到看不见节点的嵌入功能。 通过将节点特征整合到学习算法中，我们可以同时学习每个节点邻域的拓扑结构以及节点特征在邻域中的分布。我们的方法还可以利用所有图形中存在的结构特征（例如节点度）。因此，我们的算法也可以应用于没有节点特征的图。  
GraphSAGE示例和聚合方法的可视化图示如下：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/Inductive%20Representation%20Learning%20on%20Large%20Graphs/1.jpg)   
我们方法背后的关键思想是，我们将学习如何从节点的本地邻域中汇总要素信息。
1.嵌入生成（即前向传播）算法：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/Inductive%20Representation%20Learning%20on%20Large%20Graphs/2.jpg)  
算法1的直觉是，在每次迭代或搜索深度处，节点都会聚合来自其本地邻居的信息，并且随着此过程的反复进行，节点将从图的更远范围逐渐获取越来越多的信息（从一阶到多阶）。假设我们要聚合K次，则需要有K个聚合函数（aggregator），可以认为是N层。每一次聚合，都是把上一层得到的各个node的特征聚合一次，在假设该node自己在上一层的特征，得到该层的特征。如此反复聚合K次，得到该node最后的特征。最下面一层的node特征就是输入的node features。  
2.GraphSAGE的参数学习    
在上面的过程中，我们需要学习各个聚合函数的参数，因此需要设计一个损失函数。损失函数是设计是根据目标任务来的，可以是无监督的，也可以是有监督的。对于无监督学习，我们设计的损失函数应该让临近的节点的拥有相似的表示，反之应该表示大不相同。所以损失函数是这样的：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/Inductive%20Representation%20Learning%20on%20Large%20Graphs/3.jpg)  
对于有监督学习，可以直接使用cross-entropy。
3. 聚合函数的选择  
这里作者提供了三种方式：  
Mean aggregator ：  
直接取邻居节点的平均。  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/Inductive%20Representation%20Learning%20on%20Large%20Graphs/5.jpg)  
LSTM aggregator：  
使用LSTM来encode邻居的特征。这里忽略掉邻居之间的顺序，即随机打乱，输入到LSTM中。作者的想法是LSTM的表示能力比较强。
Pooling aggregator：  
把各个邻居节点单独经过一个MLP得到一个向量，最后把所有邻居的向量做一个element-wise的max-pooling或者什么其他的pooling。公式如下：
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/Inductive%20Representation%20Learning%20on%20Large%20Graphs/4.jpg)  
evaluation:  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/Inductive%20Representation%20Learning%20on%20Large%20Graphs/6.jpg)  
