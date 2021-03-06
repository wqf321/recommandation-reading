motivation:线性模型不能有效的学习出不明显的模式；FM和GBDT虽然能做特征组合，但是不能利用全部不同特征的组合；许多模型需要依靠特征工程和手工设计特征，并且大多数模型是浅层结构，泛化性能不好;深度学习可以学习出局部特征并进而学习出高阶特征筛。但是对于CTR问题，存在多个领域并且都是类别特征（如城市、设备类型、广告类型），他们的局部依赖性是未知的，通过DNN来学习特征表示是很有前景的。FNN通过监督学习Embedding并用FM来将稀疏特征变成Dense特征，减少维度。    
method:如图所示为一个四层的FNN结构：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Learning%20over%20Multi-field%20Categorical%20Data--A%20Case%20Study%20on%20User%20Response%20Prediction/1.png)  
输入类别特征是field-wise 的One-hot编码形式，只有一个值为1，其他为0；  
FNN以FM为底层，如上图所示，从上到下描述这个网络：输出是一个sigmoid函数，然后接上两个全连接网络（使用tanh激活函数），这里解释一下Dense层z，z=(w0,z1,z2,...,xi,...,zn)其中w0是全局标量参数，n是域field的个数，其中zi∈Rk+1是第i个域的参数向量；且  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Learning%20over%20Multi-field%20Categorical%20Data--A%20Case%20Study%20on%20User%20Response%20Prediction/2.png)  
其中start和end分别代表域的开始和结束位置，x是上面描述的one-hot编码向量；本文Z向量通过FM训练完成；除了FM层，其他层都使用RBM做权重预训练，FM权重通过SGD训练，最后使用交叉熵损失来最小化损失（这里有一个注意的地方：因为输入数据大部分为0，所以只有当输入为1的时候才会梯度更新，加快了训练速度）  
另一种表示形式：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/Deep%20Learning%20over%20Multi-field%20Categorical%20Data--A%20Case%20Study%20on%20User%20Response%20Prediction/3.png)  
conclusion:1.嵌入参数可能会受到FM的过度影响；  
2.预训练阶段引入的开销降低了效率  
3.FNN仅捕获高阶特征交互。  

疑问：既然可以通过反向传播参与参数更新，FNN的embedding层（FM部分）为何要预训练？  
