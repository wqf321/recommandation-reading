Collaborative Filtering for Implicit Feedback Datasets  
动机：一般而言，用户基于item的显性反馈（比如评分数据）是可以出显示出用户对某样item的喜好程度的。但是现实生活中其实还存在着很多的隐性反馈（比如购买记录、浏览记录，搜索记录等）这样子的数据量其实是有非常的多的，但这一类数据普遍都存在一个缺陷，即它们是很难有证据显示出用户对该item的不喜欢程度，至少大部分是，毕竟用户的打分才是最直接表示喜好的一种行为。所以如何处理隐性数据，以及如何显示出用户对item的喜好程度，成为了本文要研究的问题。   
挑战：1.没有负反馈。在隐性反馈数据中，最重要的是处理消失的最能反应用户负面反馈的数据。  
2.隐性数据中存在干扰噪音。  
3.隐性数据可以反映出用户的置信程度，一般来说，一个重复发生的行为更可能反应用户的观点。  
4.应该用更好的评估方法去评价隐性反馈。对于隐式模型，我们必须考虑物品的可用性，物品与其他物品的竞争以及重复的反馈。  
方法：与显性反馈(values)，表示为rui不同，隐性反馈数据是不能显示用户的喜好程度的，本文将隐性反馈数据转换成了两个维度，一个是喜好程度(preference)，表示为pui,另一个是置信程度(confidence),表示为cui，以此，得出的分数，我们是可以看出用户的喜好程度pui的与置信程度cui的配对，然后再使用SVD方法。
Cui=1+αrui，cui会随着rui的增大而增大，增长率由α控制，经过实验后设置为40。
loss如下图  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Collaborative%20Filtering%20for%20Implicit%20Feedback%20Datasets/2.jpg)  
注：该loss与显性反馈数据的矩阵分解的目标函数相似，不能混淆，有两点重要的差别：1.注意u、i的域，是所有数据而非显性数据
2.我们需要考虑不同的置信度等级。 该Loss函数通过交叉最小二乘法进行优化。   
作者提出，他的方法其实也是可以有进一步的变形的，比如
1.可以设定rui的最小阈值，从而设定pui=0
2.改变cui的计算方法 如下图  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Collaborative%20Filtering%20for%20Implicit%20Feedback%20Datasets/1.jpg)

改进：1.一般的评分高数据，很多时候并不能够显示该数的真实情况，隐性数据有噪声，所以作者进一步把rui变成了两个可以解释的维度,一个是p ui,表示用户的喜好程度，一个是cui表示用户喜好程度的置信程度。这更好的反应了数据的本质，以及对于提高数据准确率是非常重要的。  
2.算法可以用线性时间计算完   
评估方式： 为u推荐i得到的百分等级排名，使用的评价指标是rankrankrank的平均值，即  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Collaborative%20Filtering%20for%20Implicit%20Feedback%20Datasets/3.jpg)这个指标越低越好，原文rank=50%,复现结果应大于等于50%。
个人认为 难点：   
不足：    
