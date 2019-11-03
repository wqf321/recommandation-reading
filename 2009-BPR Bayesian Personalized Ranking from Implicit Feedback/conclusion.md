动机：以往的方法都不是对物品排名做直接的优化。作者认为推荐任务本质上是一个ranking任务，以往这些方法（矩阵分解（MF）和适应性K最近邻（KNN））都没有直接优化排序模型的参数。它们进行优化的模型是用来预测某个用户是否选择了某个项目。这种优化方法不够直接。一般的推荐算法是强调用户对项目的打分，只存在用户和单个项目的关系，不去考虑两个项目对用户的影响力。而BPR则从u,i,j出发来求解u,i的大小。  
主要贡献：1.提出了一个基于最大后验估计的最佳个性化排序的一般优化准则BPR-Opt方法，展示了ROC曲线下面积的最大化的相似性。  
2.提出了一种基于随机梯度下降和训练三元组自举采样的通用学习算法LearnBPR。  
3.展示了如何将LearnBPR算法应用于两个最先进的推荐器模型。  
4.实验表明，对于个性化排名的任务，使用BPR学习模型优于其他学习方法。  
方法：BPR推荐模型：
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-BPR%20Bayesian%20Personalized%20Ranking%20from%20Implicit%20Feedback/1.jpg)    
？表示的是用户无签到数据（比如用户和商品，这里的签到可以广义的理解为没有发生浏览行为或者是购买行为），有两种情况，第一种是可能本身就是negative value，也就是说用户对item是不感兴趣的。第二种情况则是缺失值，可能发生了浏览或者是购买行为但是却丢失了，当我们在上图中从左图转换为右图时，遇到0值时并不能确定究竟是上面所描述的哪一种情况。+表示u相对于项目j更倾向于项目i，-表示u相对于项目i更倾向于项目j。  
BPR推荐系统会考虑positive value 和negative value,也就说所有item都会被个性化ranking,即使用户对某个item缺失值这个item也能够被ranking，而不是仅仅用negative value代替缺失值。
具体来说：  
用户u对i是positive value，对j是negative value,那么就说明用户相对于项目j更喜欢项目i。  
用户u对i是negative value,对j是negative value,那么是无法评估用户更喜欢项目i还是项目j的。  
用户u对i是positive value,对j是positive value,那么也是无法判断用户更喜欢项目i还是项目j的。  
BPR-OPT的推导：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-BPR%20Bayesian%20Personalized%20Ranking%20from%20Implicit%20Feedback/2.jpg)  
theta 为（用户潜在特征矩阵P，项目潜在特征矩阵Q)，>u为用户u的偏序关系，即Ii-Ij矩阵  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-BPR%20Bayesian%20Personalized%20Ranking%20from%20Implicit%20Feedback/3.jpg)  
注： ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-BPR%20Bayesian%20Personalized%20Ranking%20from%20Implicit%20Feedback/4.jpg)  

由上述几式得出：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-BPR%20Bayesian%20Personalized%20Ranking%20from%20Implicit%20Feedback/5.jpg)  

BPR Learning Algorithm：上式对theta求导得出![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-BPR%20Bayesian%20Personalized%20Ranking%20from%20Implicit%20Feedback/6.jpg)  通常，全梯度下降（FGD）方法虽然会导致“正确”方向下降，但是收敛速度很慢。由于我们在DS中对O（| S || I |）进行了三重训练，因此在每个更新步骤中计算全梯度是不可行的。此外，为了优化具有全梯度消散的BPR-Opt，训练对中的偏斜也会导致收敛不佳。
因此使用随机梯度下降算法（SGD），得出收敛算法伪代码如下图：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2009-BPR%20Bayesian%20Personalized%20Ranking%20from%20Implicit%20Feedback/7.jpg)

评估：本文BPR与其他推荐学习方法进行了比较，选择了矩阵分解（MF）和k最近邻（kNN）这两个受欢迎的模型类别。对于任何非个性化排名方法，都给出了AUC（NPmax）的理论上限


