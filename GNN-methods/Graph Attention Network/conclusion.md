motivation:GCN中的谱方法存在的问题：  
1.必须基于相应的图结构才能学到拉普拉斯矩阵L （训练局限性） 
2.对于一个图结构训练好的模型，不能运用于另一个图结构（所以此文称自己为半监督的方法）  
每个节点的相邻链接数量是不一样的，如何能设置相应的卷积尺寸来保证CNN能够对不同的相邻节点进行操作是必须要研究的。前人运用GraphSAGE, 取得了不错的结果，这种方法是将相邻节点设置为固定的长度，然后进行specific aggregator，例如mean，sum，lstm等。  
近两年发展起来的attention机制，可以处理任意大小输入的问题，并且关注最具有影响能力的输入。  
contribution:1.引入masked self-attentional layers 来改进前面图卷积graph convolution的缺点  
2.对不同的相邻节点分配相应的权重，既不需要矩阵运算，也不需要事先知道图结构  
3.四个数据集上达到state of the art的准确率Cora、Citeseer、Pubmed、protein interaction  
method:思想：针对每一个节点运算相应的隐藏信息，在运算其相邻节点的时候引入注意力机制：
1.高效：针对相邻的节点对，并且可以并行运算   
2.灵活：针对有不同度（degree）的节点，可以运用任意大小的weight与之对应。（这里我们解释一个概念，节点的度degree：表示的是与这个节点相连接的节点的个数）  
3.可移植：可以将模型应用于从未见过的图结构数据，不需要与训练集相同。  
具体推导见文：  
https://blog.csdn.net/weixin_36474809/article/details/89401552  

https://blog.csdn.net/b224618/article/details/81407969（辅助理解）
