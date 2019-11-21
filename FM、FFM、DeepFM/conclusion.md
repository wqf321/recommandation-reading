注：对FM、FFM、DeepFM系列做整理与总结  
引：在传统的线性模型中，每个特征都是独立的，如果需要考虑特征与特征之间的相互作用，可能需要人工对特征进行交叉组合。非线性SVM可以对特征进行核变换，但是在特征高度稀疏的情况下，并不能很好的进行学习。现在有很多分解模型可以学习到特征之间的交互隐藏关系，基本上每个模型都只适用于特定的输入和场景。推荐系统是一个高度系数的数据场景，由此产生了FM系列算法。
参考文章：https://blog.csdn.net/hiwallace/article/details/81333604   
https://www.jianshu.com/p/6f1c2643d31b  
https://www.jianshu.com/p/152ae633fb00  
https://www.jianshu.com/p/781cde3d5f3d  
https://www.cnblogs.com/ljygoodgoodstudydaydayup/p/6340129.html(这里补充解释了loss的推导过程)  
https://blog.csdn.net/zynash2/article/details/79348540
1.FM（Factorization Machines）
FM(Factorization Machine)主要是为了解决数据稀疏的情况下，特征怎样组合的问题。已一个广告分类的问题为例，根据用户与广告位的一些特征，来预测用户是否会点击广告。数据如下：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/1.png)  
clicked是分类值，表明用户有没有点击该广告。1表示点击，0表示未点击。而country,day,ad_type则是对应的特征。对于这种categorical特征，一般都是进行one-hot编码处理。将上面的数据进行one-hot编码以后，就变成了下面这样 ：  ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/2.png)   
因为是categorical特征，所以经过one-hot编码以后，不可避免的样本的数据就变得很稀疏。举个非常简单的例子，假设淘宝或者京东上的item为100万，如果对item这个维度进行one-hot编码，光这一个维度数据的稀疏度就是百万分之一。由此可见，数据的稀疏性，是我们在实际应用场景中面临的一个非常常见的挑战与问题。  
对特征进行组合：  普通的线性模型，我们都是将各个特征独立考虑的，并没有考虑到特征与特征之间的相互关系。但实际上，大量的特征之间是有关联的。最简单的以电商为例，一般女性用户看化妆品服装之类的广告比较多，而男性更青睐各种球类装备。那很明显，女性这个特征与化妆品类服装类商品有很大的关联性，男性这个特征与球类装备的关联性更为密切。如果我们能将这些有关联的特征找出来，显然是很有意义的。一般的线性模型为：  ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/3.png)   
从上面的式子很容易看出，一般的线性模型压根没有考虑特征间的关联。为了表述特征间的相关性，我们采用多项式模型。在多项式模型中，特征xi与xj的组合用xixj表示。为了简单起见，我们讨论二阶多项式模型。具体的模型表达式如下：  ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/4.png)  上式中，n表示样本的特征数量,xi表示第i个特征。
与线性模型相比，FM的模型就多了后面特征组合的部分。   
求解MF：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/5.png)  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/6.png)  
经过这样的分解之后，我们就可以通过随机梯度下降SGD进行求解：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/7.png)  
2.FFM(Field-aware Factorization Machine)  
在CTR预估中，经常会遇到one-hot类型的变量，one-hot类型变量会导致严重的数据特征稀疏的情况，为了解决这一问题，在上一讲中，我们介绍了FM算法。这一讲我们介绍一种在FM基础上发展出来的算法-FFM（Field-aware Factorization Machine）。在CTR预估中，通常会遇到one-hot类型的变量，会导致数据特征的稀疏。未解决这个问题，FFM在FM的基础上进一步改进，在模型中引入类别的概念，即field。将同一个field的特征单独进行one-hot，因此在FFM中，每一维特征都会针对其他特征的每个field，分别学习一个隐变量，该隐变量不仅与特征相关，也与field相关。
假设样本的n个特征属于f个field，那么FFM的二次项有nf个隐向量。而在FM模型中，每一维特征的隐向量只有一个。FM可以看做FFM的特例，把所有特征都归属到一个field的FFM模型。其模型方程为：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/8.png)  
这条记录可以编码成5个特征，其中“Genre=Comedy”和“Genre=Drama”属于同一个field，“Price”是数值型，不用One-Hot编码转换。为了方便说明FFM的样本格式，我们将所有的特征和对应的field映射成整数编号。  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/9.png)   
那么，FFM的组合特征有10项，如下图所示。  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/10.png)   
其中，红色是field编号，蓝色是特征编号。  
3.DeepFM:  
目前的CTR预估模型，实质上都是在“利用模型”进行特征工程上狠下功夫。传统的LR，简单易解释，但特征之间信息的挖掘需要大量的人工特征工程来完成。由于深度学习的出现，利用神经网络本身对于隐含特征关系的挖掘能力，成为了一个可行的方式。DNN本身主要是针对于高阶的隐含特征，而像FNN（利用FM做预训练实现embedding，再通过DNN进行训练，有时间会写写对该模型的认识）这样的模型则是考虑了高阶特征，而在最后sigmoid输出时忽略了低阶特征本身。鉴于上述理论，目前新出的很多基于深度学习的CTR模型都从wide、deep（即低阶、高阶）两方面同时进行考虑，进一步提高模型的泛化能力，比如DeepFM。  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/11.png)   
可以看到，整个模型大体分为两部分：FM和DNN。简单叙述一下模型的流程：借助FNN的思想，利用FM进行embedding，之后的wide和deep模型共享embedding之后的结果。DNN的输入完全和FNN相同（这里不用预训练，直接把embedding层看作一层的NN），而通过一定方式组合后，模型在wide上完全模拟出了FM的效果，最后将DNN和FM的结果组合后激活输出。FM部分的详细结构如下：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/12.png)   
FM部分是一个因子分解机。关于因子分解机可以参阅文章[Rendle, 2010] Steffen Rendle. Factorization machines. In ICDM, 2010.。因为引入了隐变量的原因，对于几乎不出现或者很少出现的隐变量，FM也可以很好的学习。FM的输出公式为：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/13.png)   
深度部分：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/14.png)   
深度部分是一个前馈神经网络。与图像或者语音这类输入不同，图像语音的输入一般是连续而且密集的，然而用于CTR的输入一般是及其稀疏的。因此需要重新设计网络结构。具体实现中为，在第一层隐含层之前，引入一个嵌入层来完成将输入向量压缩到低维稠密向量。  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/15.png)   
嵌入层(embedding layer)的结构如上图所示。当前网络结构有两个有趣的特性，1）尽管不同field的输入长度不同，但是embedding之后向量的长度均为K。2)在FM里得到的隐变量Vik现在作为了嵌入层网络的权重。这里的第二点如何理解呢，假设我们的k=5，首先，对于输入的一条记录，同一个field 只有一个位置是1，那么在由输入得到dense vector的过程中，输入层只有一个神经元起作用，得到的dense vector其实就是输入层到embedding层该神经元相连的五条线的权重，即vi1，vi2，vi3，vi4，vi5。这五个值组合起来就是我们在FM中所提到的Vi。在FM部分和DNN部分，这一块是共享权重的，对同一个特征来说，得到的Vi是相同的。  
