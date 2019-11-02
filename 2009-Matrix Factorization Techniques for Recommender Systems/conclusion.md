注：此文为综述性文章，主要叙述推荐系统中现有的矩阵分解技术
主要内容与工作：  
1、介绍基于内容的推荐系统原理，并举了音乐网站Pandora.com的例子：  
推荐系统依赖于不同类型的输入数据，在这些输入数据中，其中一个维度表示用户，另一个维度表示感兴趣的项目。推荐系统基于两种策略，一种是基于内容过滤的推荐算法，另一种是基于协同过滤推荐算法。协同过滤的两个主要模型是邻域模型和隐因子模型。  
2、介绍协同过滤算法，并将其分为neighborhood methods 和 latent factor models两类。着重介绍了矩阵分解的原理。  
3、矩阵分解的一个优点是它允许合并额外的信息。当无法获得明确反馈时，推荐系统可以使用隐式反馈推断用户偏好，隐式反馈通过观察用户行为（包括购买历史记录、浏览历史记录、搜索模式甚至鼠标移动）间接反映意见。隐式反馈通常表示事件的存在或不存在，因此它通常由一个密集的矩阵表示。  
4、具体介绍了几种矩阵分解算法：  
1.Basic MF：基本的矩阵分解，向量q_i∈R^f, p_u∈R^f,则评分预测为：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-Matrix%20Factorization%20Techniques%20for%20Recommender%20Systems/1.jpg)  
，为防止过拟合采用正则化平方误差  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-Matrix%20Factorization%20Techniques%20for%20Recommender%20Systems/2.jpg)  
预测值接近真实值就是使其误差越小，这是我们的目标函数（损失函数），我们的目标就是使误差达到最小化，最小化上述公式的常用方法是梯度下降法和最小二乘法。这里比较了两种方法各自的优缺点。  
SGD比ALS更容易和更快，但是ALS在以下两种情况下是有利的:  
a.系统可以使用并行化，在ALS中，系统独立于其他项目因子计算每个qi，并独立于其他用户因素计算每个pu。这导致了算法的潜在大规模并行化；  
b.针对以隐式数据为中心的系统，因为训练集不能被认为是稀疏的，所以在每个训练案例上循环，如梯度下降那样，是不切实际的。ALS可以有效处理这种情况。  
2.biases MF：  ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-Matrix%20Factorization%20Techniques%20for%20Recommender%20Systems/4.jpg)
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-Matrix%20Factorization%20Techniques%20for%20Recommender%20Systems/5.jpg)    
3.嵌入额外信息的MF: ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-Matrix%20Factorization%20Techniques%20for%20Recommender%20Systems/6.jpg)    
4.时序动态MF:![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-Matrix%20Factorization%20Techniques%20for%20Recommender%20Systems/7.jpg)  
5.考虑隐式反馈的MF: ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-Matrix%20Factorization%20Techniques%20for%20Recommender%20Systems/8.jpg)    
额外介绍了Netflix推荐大赛的情况  

总结：  
矩阵分解技术已成为协同过滤推荐器中的主要方法。Netflix Prize数据等数据集的经验表明，它们的准确性优于传统的近邻技术。同时，它们提供了一个紧凑的内存高效模型，系统可以相对容易地学习。使这些技术更加方便的原因是模型可以自然地整合数据的许多关键方面，例如多种形式的反馈，时间动态和置信水平。  


