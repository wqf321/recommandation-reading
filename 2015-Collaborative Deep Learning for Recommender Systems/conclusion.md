motivation：CF有他的局限性，当打分（rating）很稀疏时，预测精度会下降很厉害，同时，新产品的冷启动也是CF的问题。因此，近年来，混合方法应用比较广。协同话题回归（CTR）是当下被推崇的紧耦合方法。它是一个整合了Topic model，LDA，CF，概率矩阵分解（PMF）的概率图模型。本文提出的CDL方法就是把CRT模型和深度学习模型SDAE集合起来，形成一个多层贝叶斯模型。  
作者用贝叶斯法则表征栈式自编码器（sdae），用深度学习的方式表示content information和rating matrix，使两者双向相互影响。SDAE在CDL中用于学习特征，实际上更多的深度学习模型可以用进来，如Deep Boltzmann machines,RNN, CNN.  
  
challenges：1.以往方法同时涉及深度学习和CF，但它们实际上属于基于CF的方法，因为它们没有像CTR那样包含内容信息，这对于准确推荐至关重要。  
2.对于音乐推荐，以往方法直接使用常规的CNN或深度置信网络（DBN）辅助表示内容信息的表示学习，但是他们的模型的深度学习组件是确定性的，没有对噪声建模，因此它们的鲁棒性较差。  
3.CNN直接链接到评分矩阵，这意味着当评分稀疏时，模型的效果会很差。  
  
contribution：1.CDL可抽取content的深度特征，并捕获content或者user的相似度。这种学习方式不仅可以用于推荐，也可以用于别的地方。  
2.学习目标不是简单的分类或者reconstruction，本文的目标是通过概率框架用CF做一个更复杂的目标。  
3.使用了最大后验估计(MAP)，CDL贝叶斯法则的抽样，贝叶斯版本的反向传播。  
4.CDL是第一个在最新的深度学习模型和RS之间架起桥梁的层次化贝叶斯模型。此外，由于其贝叶斯性质，CDL可以轻松扩展以合并其他辅助信息以进一步提高性能。  

method:1.堆叠式降噪自动编码器（stacked Denoising Autoencoders——SDAE）：SDAE是一个前馈神经网络，用于通过学习预测输出中的干净输入本身来学习输入数据的表示(encoding)，如下图所示。通常中间的隐藏层，即图中的X2，被限制为瓶颈，而输入层X0是干净输入数据的损坏版本。
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2015-Collaborative%20Deep%20Learning%20for%20Recommender%20Systems/1.jpg)  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2015-Collaborative%20Deep%20Learning%20for%20Recommender%20Systems/2.jpg)  
下图表示了该文中SDAE的优化目标：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2015-Collaborative%20Deep%20Learning%20for%20Recommender%20Systems/3.jpg)  

2.一般化的贝叶斯SDAE：其中，干净输入Xc和噪声X0都是可观察的。![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2015-Collaborative%20Deep%20Learning%20for%20Recommender%20Systems/4.jpg)  
当lamda-S趋近于无穷大时，式(1)中的高斯分布变成Dirac delta分布中心点在sigma(Xl-1,j*Wl+bl)，
模型会退化成一个SDAE高斯公式。因此，我们叫他推广Bayesian SDAE.注意: 前L/2 层网络扮演encoder，后L/2层网络扮演decoder。当权重延迟纳入考虑，最大后验概率等同于最小化重建误差。
3.Collaborative Deep Learning-CDL
用Bayesian SDAE作为成分，组成CDL的过程如下：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2015-Collaborative%20Deep%20Learning%20for%20Recommender%20Systems/5.jpg)    
上述lamda-w、lamda-n、lamda-u、lamda-s、lamda-v、是寄生参数，Cij是置信度参数。中间层X-l/2充当rating和content的桥梁。带有主题偏执sigma-j的中间层是开启学习特征和捕获相似度的钥匙。如同推广SDAE中，这里也可以让趋于无穷大,当趋近于正无穷，CDL的网络就如最上面的图1所示。  
4.最大后验估计：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2015-Collaborative%20Deep%20Learning%20for%20Recommender%20Systems/6.jpg)    

evaluation:本文主要使用了三个数据集（citeulike-a、citeulike-t、最后一个数据集Netflix由两部分组成。第一部分，带有收视率和电影标题，来自Netflix chal-lenge数据集。第二部分，包括对应电影的情节，是作者自行从IMDB4收集的）对于每个数据集，随机选择与每个用户关联的项目以形成训练集，并将其余的数据集用作测试集。为了评估和比较稀疏设置和密集设置下的模型，在实验中分别将Pto设置为1和10。对于每个P值，使用随机选择的不同训练集重复评估五次，并报告平均表现。

个人认为：  
难点：最大后验估计部分的推导没太看明白。。。
