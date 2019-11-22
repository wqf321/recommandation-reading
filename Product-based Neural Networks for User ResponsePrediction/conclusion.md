motivation:面对极端稀疏性，传统模型可能会限制从数据中挖掘浅层模式(即低阶特征组合)的能力。认为在embedding输入到MLP之后学习的交叉特征表达并不充分，另一方面，深层神经网络等深层模型由于输入数据巨大的特征空间，不能直接输入到网络中。因此要提出PNN，一个嵌入层，用于学习分类数据的分布式表示；产品层，用于捕获字段间类别之间的交互模式；进一步的全连接层，用于探索高阶特征交互。  
challenges:1.以往的ML模型（LR、FM等），高度依赖特征工程来捕捉高阶的潜在模式。   
2.传统的DNN模型在应用时，嵌入初始化的质量很大程度上受到FM的限制。更重要的是，感知器层的“添加”操作可能不适用于探索多个field中分类数据的交互（这里没太看懂）。   
contribution:1.PNN网络从一个不需要预训练的embedding layer开始。  
2.基于嵌入式特征向量构建了一个product layer，以对场间特征交互进行建模。  
3.进一步提炼出具有完全连接的MLP的高阶特征模式。  
method:  1.PNN：  
PNN模型的结构如图所示：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Product-based%20Neural%20Networks%20for%20User%20ResponsePrediction/1.png)  
PNN输出为CTR预测值yhat（0，1）：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Product-based%20Neural%20Networks%20for%20User%20ResponsePrediction/2.png)  
L2layer  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Product-based%20Neural%20Networks%20for%20User%20ResponsePrediction/3.png)  
L1layer  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Product-based%20Neural%20Networks%20for%20User%20ResponsePrediction/4.png)  
L1公式中，Lz为线性信号、Lp为二次信号。  
由此可以引出product layer：   
product思想来源于，在ctr预估中，认为特征之间的关系更多是一种and“且”的关系，而非add"加”的关系。例如，性别为男且喜欢游戏的人群，比起性别男和喜欢游戏的人群，前者的组合比后者更能体现特征交叉的意义。   
product layer可以分成两个部分，一部分是线性部分lz，一部分是非线性部分lp。二者的形式如下：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Product-based%20Neural%20Networks%20for%20User%20ResponsePrediction/5.png)  
其中lz是线性信号向量，因此我们直接用embedding层得到：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Product-based%20Neural%20Networks%20for%20User%20ResponsePrediction/7.png)  
论文中使用的等号加一个三角形，其实就是相等的意思，可以认为z就是embedding层的复制。  
对于p来说，这里需要一个公式进行映射：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Product-based%20Neural%20Networks%20for%20User%20ResponsePrediction/8.png)  
不同的g的选择使得我们有了两种PNN的计算方法，一种叫做Inner PNN，简称IPNN，一种叫做Outer PNN，简称OPNN。  
接下来，我们分别来具体介绍这两种形式的PNN模型，由于涉及到复杂度的分析，所以我们这里先定义Embedding的大小为M，field的大小为N，而lz和lp的长度为D1。  
注：此部分可以参阅https://www.jianshu.com/p/be784ab4abc2  
文中定义的矩阵的点乘：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Product-based%20Neural%20Networks%20for%20User%20ResponsePrediction/6.png)  
总的来说，PNN使用产品层来探索特征交互。向量乘积可以被视为一系列加法/乘法运算。内积和外积只是两个实现。  
