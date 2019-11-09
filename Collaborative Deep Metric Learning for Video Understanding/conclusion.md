motivation：研究人员已经处理了各种领域的视频任务，包括视频分类、搜索、个性化推荐等等。然而，在将这些领域合并到一个统一的学习框架中存在研究空白。为此，我们提出了一个深层网络，它利用视频的视听内容将视频嵌入到一个保持视频与视频关系的度量空间中.  
contributions:1,训练了从视频内容到CF信号的映射，建立了视频之间的高级语义关系。这种内容感知嵌入可以用于解决冷启动问题.  
2.验证了文中的嵌入可以推广，并且可以转移到各种问题，包括个性化视频推荐和视频注释   
3.提出的方法是可扩展的，可以部署在像YouTube这样的大规模视频共享平台上。描述了关于可扩展性问题的工程问题和解决方案，并在大规模数据集上进行了评估
method:1.how to extract video and audio features:下图a部分说明了如何提取视频特征
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Collaborative%20Deep%20Metric%20Learning%20for%20Video%20Understanding/1.jpg) 
