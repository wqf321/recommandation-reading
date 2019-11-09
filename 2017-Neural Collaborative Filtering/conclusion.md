motivation:近年来，深层神经网络在语音识别，计算机视觉和自然语言处理方面都取得了巨大的成功。然而相对的，对应用深层神经网络的推荐系统的探索却受到较少的关注。所以本文探讨了使用深层神经网络来学习数据的交互函数，而不是那些以前已经完成的手动工作（handcraft）。  
contribution:  
1、提出了一种神经网络结构来模拟用户和项目的潜在特征，并设计了基于神经网络的协同过滤的通用框架NCF。  
2、表明MF可以被解释为NCF的特例，并利用多层感知器来赋予NCF高水平的非线性建模能力。  
3、对两个现实世界的数据集进行广泛的实验，以证明我们的NCF方法的有效性和对使用深度学习进行协作过滤的承诺。  
method:
1.符号说明  M 和 N 分别为项目和用户的数目。Y∈RM×N 表示从用户的隐性反馈得到的用户-项目交互矩阵，定义如下：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2017-Neural%20Collaborative%20Filtering/2.jpg）这里，yui 值为 1仅表示用户 u 与项目 i 之间有交互信息，并不意味 u 喜欢 i；同样的yui 值为 0 也并不表示u 讨厌 i。这其中缺少负反馈信息。这也是隐性反馈的一个挑战，因为它给用户偏好信息带来了干扰。隐式反馈的推荐任务可以表示为预测矩阵 Y 中未观察到交互的entry的分数y^-ui,用以对整体商品做排名
2.矩阵分解  
令pu 和 qi 分别表示用户 u 和项目 i 的潜在向量，MF用pu 和 qi 的内积来评估它们之间的交互.如下图：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2017-Neural%20Collaborative%20Filtering/3.jpg）    
下图展示了MF在低维潜在空间表示用户-项目复杂的交互的局限性。  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2017-Neural%20Collaborative%20Filtering/1.jpg）  

注意：（1）由于MF将用户和项目映射到同一潜在空间中，两个用户之间的相似性也可以用内积，或者潜在向量之间的角度的余弦值来衡量。（2）使用Jaccard系数作为MF需要恢复的两个用户的真实状况之间的相似度，令 Ru 表示与用户 u 交互的项目集，那么用户 u 和 j 之间的Jaccard相似系数就被定义为：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2017-Neural%20Collaborative%20Filtering/5.jpg）  

3.NCF框架  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2017-Neural%20Collaborative%20Filtering/4.jpg）  
上图中输入层（Input Layer）上面是嵌入层（Embedding Layer）;它是一个全连接层，用来将输入层的稀疏表示映射为一个密集向量（dense vector）。这些嵌入后的向量其实就可以看做是用户（项目）的潜在向量。然后将这些嵌入向量送入多层网络结构，最后得到预测的分数。NCF层的每一层可以被定制，用以发现用户-项目交互的某些潜在结构。最后一个隐层 X 的维度尺寸决定了模型的能力。最终输出层是预测分数y^ui，文中，训练目标是最小化 y^ui和其目标值yui之间逐点损失（point-wise loss）.  

本文将NCF预测模型表示为：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2017-Neural%20Collaborative%20Filtering/6.jpg）
其中 ϕout 和 ϕx 分别表示为输出层和第 x 个neural CF层映射函数，总共有 X 个neural CF层
4.NCF的目标函数
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2017-Neural%20Collaborative%20Filtering/7.jpg）
这是NCF方法需要去最小化的目标函数，并且可以通过使用随机梯度下降（SGD）来进行训练优化。这个函数和二类交叉熵损失函数（binary cross-entropy loss，又被成为log loss）是一样的。通过在NCF上使用这样一个概率处理（probabilistic treatment），把隐性反馈的推荐问题当做一个二分类问题来解决。 对于消极实例 ν− ，每次迭代均匀地从未观察到的相互作用中采样（作为消极实例）并且对照可观察到交互的数量，控制采样比率。

5.GMF：这里就是将用 PTVUu 表示用户的潜在向量 pu ，QTViI 表示项目的潜在向量qi ,定义第一层神经CF层的映射函数为pu·qi。
如果将 aout 看做一个恒等函数， h 权重全为 1，显然这就是本文的MF模型。
6.MLP:用MLP（多层感知机）来学习用户和项目潜在特征之间的相互作用。NCF框架下，MLP模型定义为：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2017-Neural%20Collaborative%20Filtering/8.jpg）
7.GMF与MLP结合
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/2017-Neural%20Collaborative%20Filtering/9.jpg）
