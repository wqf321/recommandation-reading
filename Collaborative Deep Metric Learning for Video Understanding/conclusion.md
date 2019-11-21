motivation：研究人员已经处理了各种领域的视频任务，包括视频分类、搜索、个性化推荐等等。然而，在将这些领域合并到一个统一的学习框架中存在研究空白。为此，我们提出了一个深层网络，它利用视频的视听内容将视频嵌入到一个保持视频与视频关系的度量空间中.  
contributions:1,训练了从视频内容到CF信号的映射，建立了视频之间的高级语义关系。这种内容感知嵌入可以用于解决冷启动问题.  
2.验证了文中的嵌入可以推广，并且可以转移到各种问题，包括个性化视频推荐和视频注释   
3.提出的方法是可扩展的，可以部署在像YouTube这样的大规模视频共享平台上。描述了关于可扩展性问题的工程问题和解决方案，并在大规模数据集上进行了评估
method:  
1.how to extract video and audio features:下图1（a）部分说明了如何提取视频特征    
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Collaborative%20Deep%20Metric%20Learning%20for%20Video%20Understanding/1.jpg)   
1帧的速度传输进入 Inception-v3 network，这些帧级功能通过平均池化来聚合到视频级（可能有更好的聚合方法）
出于存储和计算的原因，我们应用主成分分析(和白化)将特征维数减少到1，500。这些帧级特征通过平均池被聚集到视频级。我们使用VGG启发的声学模型和ResNet-50的改进版本提取音频特征。具体来说，音频被分成不重叠的960毫秒帧，然后用每10毫秒25毫秒窗口的短时傅立叶变换分解，产生64个熔化间隔的频谱图。
2.协同深度度量学习：  
上面提取的视觉和听觉特征简洁地表示了视频内容，但是不包含关于视频对之间关系的信息。我们为内容特征训练嵌入函数来预测协同信号。
1.训练目标。给定一个流媒体会话，比如用户在YouTube上观看视频，我们汇总来自多个用户的合作观看，以在视频上构建图表，其中边缘由合作观看频率加权。实际上，我们可能会将边缘降低到某个阈值以下。我们希望共同观看的视频在嵌入空间中显得很近。为了做到这一点，我们优化了排序三元组损失[43]，其中训练数据点被定义为三个视频的三元组：给定视频，正视频和负视频。构造三元组，以使给定与正相关，而不与正负相关，或者给定视频与正负相关，比正负更相关。
我们通过最小化下述函数来完成优化
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Collaborative%20Deep%20Metric%20Learning%20for%20Video%20Understanding/2.jpg)  
其中xai、xpi和xnico分别对应于第一个训练数据点的向量、正向量和负向量.  
2.嵌入网络：  
我们在图1（b）中提出了两种可能的形式：早期融合和晚期融合。第一个将输入端的两种模式（视觉和音频）连接起来，第二种在单独的完全连接的层之后将它们组合在一起。还有其他选择，例如中级融合，但我们仅对上述两种进行试验。在这两种情况下，我们都在输出端应用L2归一化。训练嵌入网络的参数以最小化等式（1）中的目标。
4.experiments：
1.相关视频检索：a.归一化贴现累计收益(NDCG)考虑列表中检索项目的顺序  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Collaborative%20Deep%20Metric%20Learning%20for%20Video%20Understanding/3.jpg) 
平均平均精度（MAP）是精度调用曲线下的面积，由下式给出  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Collaborative%20Deep%20Metric%20Learning%20for%20Video%20Understanding/4.jpg) 
2.视频推荐：个性化视频推荐是按照特定用户的偏好对项目进行排名的任务。它类似于相关的视频检索，只是这里的查询(上下文)是用户而不是单个视频。我们用一个用户最近的比赛历史，即一系列视频，来为她建模。计算与候选视频的相似度现在给出一系列分数，每个分数对应一个最近观看的视频。我们想把这些分数合为一。一种可能的聚合是算术平均值，其中排名函数返回得分最大的视频:![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Collaborative%20Deep%20Metric%20Learning%20for%20Video%20Understanding/5.jpg) 
5.evaluation:我们使用amendment方法对MovieLens2进行评估，该修正是建议中使用最广泛的公共数据集之一。由于电影没有视频和音频数据，因此我们收集了YouTube上的电影预告片作为代替。为了获得MovieLens中电影的预告片YouTube videoID，我们以“ <MovieTitle>（<year>）预告片”的形式在ongoogle.com上搜索了MovieLens中的所有电影，并获得了YouTube的第一个结果，但结果排名前5位.通常，YouTube结果是继IMDB和Wikipedia之后的第三个结果。在某些情况下，我们的搜索查询未返回YouTube预告片。我们收集了26,744部独特电影的25,141（94.0％）个预告片，这些电影在MovieLens 20M中得到了评级。没有预告片的电影被排除在实验之外。  
  首先，我们评估第4.2.1节中讨论的聚合方法。对于每个用户，我们按5：5的比例将评分分为训练和测试分区。测试评级低于10的用户被排除在实验之外。对于每位用户，我们将其优先级电影定义为训练分区中评分高于某个阈值的电影：1）固定阈值为{3,4,5}星或更高； 2）用户自己的平均评分；和3）没有阈值（在没有明确评级的情况下，模拟具有观看历史记录的应用们的嵌入空间中按点积计算。
  我们通过比较得出结论：1.阈值越高，一般性能越好。这是有道理的，因为只要我们有足够的用户评分，我们就会以较高的阈值仅拍摄我们确定用户喜欢的视频。当我们不过滤掉收视率较低的视频，而仅在隐式反馈（例如点击次数，观看次数）的应用中看到性能下降时，这可能是唯一的选择。
  2.一般而言，最大聚合的效果要好于平均聚合。换句话说，最好将最相似的项目推荐给用户的收藏夹之一，这可能是因为许多用户拥有不止一种偏好的电影类型。我们还尝试了这些聚合的一些变体，例如topk = 2,3,5的平均值和方差，但这些均未超过最大和平均聚合。