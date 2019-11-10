motivation:面对CF时，稀疏度甚至冷启动仍然是不可避免的问题。尽管可以在当今的网站上轻松找到多模态信息，但是大多数现有的CF方法设计得不好，无法适当地将它们合并。在本文中，我们打算探索融合用户和项目的大量辅助跨媒体信息的潜在方法，以帮助增强传统CF。对于离散特征，采用嵌入来表示它们。为了合并不同的离散成分，我们使用注意力机制来自适应地学习它们各自对特定评级预测的重要性。对于连续特征，我们使用特征提取网络（例如CNN）来利用信息表示来帮助限制项目的嵌入空间，因此，改进了因子向量的可解释性。  
challenges:以往的方法提供了有效的方法来处理特定类型的辅助信息，但是几乎不能处理不同的内容形式，并且也忽略了这些特征的不平等影响。另一部分工作利用DNN来实现更复杂的非线性CF，然而这对数据稀疏没有好处。总之，辅助跨媒体信息可以通过补充用户或项目的特征表示来帮助改进推荐。但是，在考虑利用这些信息来获得最佳性能时，仍然有几个问题需要解决：  
1.辅助信息是异构的，它可以是图像、文档、不同种类的类别特征，这些类别特征可以是one-hot也可以是multi-hot。有效的特征提取和表示方法是第一步  
2.不同类型的辅助信息以及相同类型组（例如电影演员）中的不同特征可以与所描述的预测目标具有不同的相关性。推荐算法应当能够自适应地区分其权重  
3.如何混合在一起并利用传统CF的辅助功能和用户-项矢量（user-item vector）
method:1.DMM-CF框架：  
对于辅助特征，我们将它们分为两类:离散的和连续的。以电影为例，类型是离散的特征，而封面图像是连续的。对于离散特征，我们使用嵌入直接表示它们。对于图像的连续特征，我们采用CNN深度分析和提取内容特征。提出的多媒体信息深度协同过滤框架(DMM-CF)在图1中被表示：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Collaborative%20Filtering%20Incorporating%20Auxiliary%20Multi-media%20Information/1.jpg)   
首先，我们简要描述评级预测问题的可能辅助功能。给定一个特定的用户项目评分对，当前用户和项目可能分别具有一些离散的辅助信息。该项目具有多种电影类型，一些演员，一些不同的标签，一个唯一的导演，这些自然地分为不同的小领域组。项目也可以具有连续的媒体特征，由于它们独特的内容类型，通常以声音信号、图像、视频等形式，所以它们不能被直接嵌入和学习。基于离散和连续的辅助特征输入，图一可以描述所提出的增强型MF框架。
2.基于注意力的合并网络   
如上述框架所述，基于注意力的合并网络旨在根据与当前用户-项目交互对的相关性为每个不同的离散辅助特征分配自适应贡献权重，因此难以预先指定注意力权重，因此我们设计了一个多层神经网络以自适应地学习它们，在获得每个辅助特征的输出值之后，我们使用softmax函数对输出向量进行归一化，以使权重之和等于1。最后取所有辅助特征嵌入的加权和，可以表示为![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Collaborative%20Filtering%20Incorporating%20Auxiliary%20Multi-media%20Information/3.jpg)   
其中ωj是离散辅助特征与其他辅助特征的权重。注意，Vaux，ωj应该实际上是Vauxui，ω。毫无疑问，此处用于区分用户项目的下标在此之后被忽略，并且随后进行类似的处理。ω的计算公式为  ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Collaborative%20Filtering%20Incorporating%20Auxiliary%20Multi-media%20Information/4.jpg)  
下图说明基于注意力的合并网络  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Collaborative%20Filtering%20Incorporating%20Auxiliary%20Multi-media%20Information/2.jpg)   
3.连续媒体特征提取：  
在本文中，我们拍摄的图像是MovieLens数据集中的电影海报，作为描述连续媒体特征提取的栗子：  
特征提取器将图像的原始像素作为输入，并使用多层网络生成固定长度（与项目向量相同的尺寸）的输出向量。对于典型的MxN RGB图像输入，MxNx3编码的矢量能够表示所有原始像素。近年来，通过深度学习的研究，图像的识别和分类已经得到了很大的发展和改进。诸如VGG16，VGG19，ResNet50的流行模型被证明是非常成功的，已在我们的特征提取器中使用。在这里，我们提出一种混合版本：将整个网络分为结构和参数固定的固定卷积部分和涉及训练的可训练全连接部分。详细地，为了从媒体特征中提取代表性特征，首先将预训练模型的卷积层应用于原始像素向量以生成瓶颈特征。然后，使用定制的密集层对其进行进一步处理，这些密集层的输出与用户和项目潜在模型的维向量相同。提取模型的结构如图3所示。![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Collaborative%20Filtering%20Incorporating%20Auxiliary%20Multi-media%20Information/9.jpg) 
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Collaborative%20Filtering%20Incorporating%20Auxiliary%20Multi-media%20Information/5.jpg)   
4.参数计算：
学习的目标是最大程度地减少用户对额定项目的预测和实际偏好。实际上，这些偏好是通过明确的评分分数或隐式的二进制指标来衡量的。在这项工作中，我们专注于明确预测。用户项目的最终评分得分由以下公式计算得出：![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Collaborative%20Filtering%20Incorporating%20Auxiliary%20Multi-media%20Information/6.jpg)  
优化损失包括代表偏好误差和潜在向量差的两部分，可以将其形式化为：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Collaborative%20Filtering%20Incorporating%20Auxiliary%20Multi-media%20Information/7.jpg)   

evaluation:
