motivation: 线性模型被广泛地应用于回归和分类问题，具有简单、快速和可解释性等优点，但是线性模型的表达能力有限，经常需要人工选择特征和交叉特征才能取得一个良好的效果，但是线性模型的表达能力有限，经常需要人工选择特征和交叉特征才能取得一个良好的效果。同时，DNN模型可以很容易的学习到高阶特征之间的作用，并且具有很好的泛化能力。DNN增加embedding层可以很容易的解决稀疏特征的问题。因此本文将传统的线性回归模型与DNN模型联合训练，以同时达到LR的拟合能力与DNN的泛化能力。
challenges:推荐问题中一个常见的挑战即是在一个模型中同时达到拟合性（memorization，可以被宽泛地定义为获取项目或特征的频繁共现，并发掘历史数据中的相关性。）和泛化性，传统的线性模型无法达到很好的泛化性，需要引入别的模块。本文引入了DNN。
contributions:  
1.提出了一个Wide＆Deep框架，用于联合训练带有嵌入的前馈神经网络和具有特征转换的线性模型，用于输入稀疏的通用推荐系统。  
2.在GooglePlay（移动应用商店拥有超过十亿的活跃用户和超过一百万个应用）上实现的Wide＆Deep推荐系统的实施和评估。  
3.有tensorflow开源代码。  

method:  wide&deep模型主要分成两部分，wide部分就是传统的LR模型，deep部分就是DNN，整个模型的结构如下图：
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2016-Wide%20%26%20Deep%20Learning%20for%20Recommender%20Systems/1.jpg)   
最左边是传统的LR模型，最右边是DNN模型，中间的是将LR和DNN结合起来的wide&deep模型。
wide部分即是传统的LR模型：y=Wt·x+b
在实际中往往需要交叉特征，对于这部分定义如下：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2016-Wide%20%26%20Deep%20Learning%20for%20Recommender%20Systems/3.jpg)    
用ϕk表示第k个交叉特征，Cki表示是第k个交叉特征的一部分。  
deep部分就是一个普通的神经网络结构，只不过在这个网络中增加embedding层用来将稀疏、高维的特征转换为低维、密集的实数向量，可以有效地解决维度爆炸。先将原始特征经过embedding层转化后，再送入DNN的隐藏层，隐藏层之间的关系定义为：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2016-Wide%20%26%20Deep%20Learning%20for%20Recommender%20Systems/4.jpg)  
将wide部分的输出和deep部分的输出相加通过sigmoid函数输出进行预测。整个模型的预测定义如下：
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2016-Wide%20%26%20Deep%20Learning%20for%20Recommender%20Systems/5.jpg)  
wide&deep模型中，wide部分和deep部分是同时训练的，不需要单独训练任何一部分。GBDT+LR模型中GBDT需要先训练，然后再训练LR，两部分具有依赖关系，这种架构不利于模型的迭代。
Join training和ensemble training的区别：（1）ensemble中每个模型需要单独训练，并且各个模型之间是相互独立的，模型之间互相不感知，当预测样本时，每个模型的结果用于投票，最后选择得票最多的结果。而join train这种方式模型之间不是独立的，是相互影响的，可以同时优化模型的参数。（2）ensemble的方式中往往要求存在很多模型，这样就需要更多的数据集和数据特征，才能取得比较好的效果，模型的增多导致难以训练，不利于迭代。而在wide&deep中，只需要两个模型，训练简单，可以很快的迭代模型   
evaluation:本文从两方面评价了该模型的性能，1.App Acquisitions（应用购买）：对比单独用wide和deep以及wide&deep如下表 ：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2016-Wide%20%26%20Deep%20Learning%20for%20Recommender%20Systems/7.jpg)  
2.Serving Performance（服务表现）：在流量高峰时，该推荐系统每秒可处理超过1000万个应用程序。性能如下表：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2016-Wide%20%26%20Deep%20Learning%20for%20Recommender%20Systems/8.jpg)  
conclusion:  
难点：数据预处理embanding部分没有弄清楚，整个模型的预测定义的表达式没有明白为什么是简单相加，即W-wide和W-deep的权值是怎么计算的没有搞懂。  
不足：整个模型的预测定义的表达式这种简单相加的形式是否可以改进？  
