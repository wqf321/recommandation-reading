注：此篇为综述性文章，列举了近几年的推荐系统领域的研究结果，以及GNN在推荐系统上的作用，并展示了该领域的一部分研究前景。  
一.近几年推荐领域的发展与经典文章：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/1.jpg)   
以往的研究主要分为协同过滤模型和基于通用特征的模型  
注：https://blog.csdn.net/hiwallace/article/details/81333604 FM系列算法解读（FM+FFM+DeepFM）  

以往的方法存在的问题：1.对隐性反馈建模存在弱链接的问题：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/2.jpg)  
2.信息的表达能力受限：（低阶联系会占据很高比重）
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/3.jpg)  
3.这些方法必须依赖复杂的交互函数来弥补次优嵌入的不足：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/4.jpg)  
4.学习到的模型具有黑盒子天性，无法解释为什么存在偏好：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/5.jpg)   
解决办法：1.探索和利用实例之间的关系    
2.应用图学和推理技术  
二.推荐系统中的随机行走策略：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/6.jpg)  
三.推荐系统中的网络嵌入：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/7.jpg)   
四.GNN在推荐系统中的应用：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/8.jpg)  
上图为近些年GNN领域的发展
数学定义：  ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/9.jpg)   
GCN的一般步骤：  ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/CIKM2019-graph-for-recommendation/10.jpg)   
