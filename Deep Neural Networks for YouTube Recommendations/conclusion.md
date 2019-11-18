motivation:YouTube在2016年的时候，用深度网络完成了工业级的视频推荐系统，主要分为候选视频集的选择和线上的rank，虽然时间过去两年了，对我们的推荐系统仍有极强的学习参考价值。  
challenges:1.数据规模庞大，用户量大，视频也多，一些在小数据集上效果不错的算法在Youtube上效果一般。    
2.不断有大量新视频上传，需要解决视频的冷启动问题。  
3.数据有噪声，用户的行为非常稀疏且只有隐反馈，视频的描述信息混乱且不规范，因此需要算法对于噪声数据有较强的鲁棒性。  
method:整个推荐架构分为两部分，召回和排序，如图2所示。第一个蓝色的漏斗就是召回算法，从百万级数量的视频物料库中筛选出几百个视频。除了这里采用的深度学习召回算法，还可以加入其他的召回视频源，如图中的红色方格，一起送给排序算法。因为计算量大，所以召回算法不可能也没必要采用所有特征，因此召回算法只采用了用户行为和场景特征。排序算法使用了更多的特征，给每个候选视频计算一个分数，并且按照分数从高到低排序，从几百个视频里边再筛选和排序出几十个视频推荐给用户。在对算法进行评估时同时采用了离线指标和在线AB test，并且以AB test作为主要的评估指标。  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Neural%20Networks%20for%20YouTube%20Recommendations/1.jpg)    
3.1召回模块：  对于候选集的生成模块，需要从视频集中选择出与用户相关的视频。本文中作者提出将其看成一个极多分类问题，基于特定的用户U和上下文C，在时间t将指定的视频wt准确地划分到第i类中，
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Neural%20Networks%20for%20YouTube%20Recommendations/2.jpg)  
3.2召回神经网络的结构：如下图所示  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Neural%20Networks%20for%20YouTube%20Recommendations/3.jpg)  
在上面的神经网络的结构中，包含了两个阶段，分别为训练阶段和服务阶段：  
1.训练部分会得到两个部分的数据：视频的embedding_v和用户的embedding_u  
2.服务阶段直接使用上述的两个embedding，两个向量的相似度的方法在这里都可以使用。
3.3排序模块：  
Ranking部分是从候选集中进行进一步的优选，除了上述的候选集生成方法，Ranking部分可以融入更多的其他的候选集。本文作者在这个部分没有使用点击率作为问题的目标，而是使用了观看时长（watch time）。因为如果使用点击率，用户可能并没有完成观看，使用观看时长，可以更好地捕捉用户的参与（原文的意思是说：会存在“clickbait”）。在神经网络的最后一层使用的方法二分类的Logistic Regression，训练样本为：
1.正例：展示的视频被点击  
2.负例：展示的视频未被点击
3.4排序神经网络的架构如下图：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Neural%20Networks%20for%20YouTube%20Recommendations/4.jpg)  
