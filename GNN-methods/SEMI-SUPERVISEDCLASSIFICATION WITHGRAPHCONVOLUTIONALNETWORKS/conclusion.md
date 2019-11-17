motivation:从基于图的半监督学习领域以及在图上运行的神经网络的最新工作中汲取了灵感,提出了一种在图结构数据上进行半监督学习的可扩展方法，该方法基于直接作用于图的卷积神经网络的有效变体。  
challenges:对于图形数据结构中的节点半监督学习分类问题，(Zhu et al., 2003; Zhou et al., 2004; Belkin et al., 2006; Weston et al., 2012)等人在处理该问题时借助基于图的正则化形式将标签信息与图结构数据平滑的结合，其具体操作是在代价函数中加入图形化的拉普拉斯正则项如下式（1）所示：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/1.jpg)   
式（1）中，L0表示图结构中有标签的部分数据的监督代价，f(·)是一个神经网络的梯度函数，λ是权值系数，X是图结构中的节点特征向量Xi拼接得到的矩阵形式，A代表图结构数据的邻接矩阵（01二进制或者权值），Dii表示邻接矩阵Aij对j所有项求和，△-D-A表示无向图G=（V,ε）（V表示图中的N个节点集合，ε表示各个节点之间的边关系）的非标准图结构的拉普拉斯算子。但式（1）的局限性在于它依赖于图中的相连节点倾向性的有着相同坐标这个假设，而实际情况下，图中的边可能并不一定能够反应出节点之间的相似性而可能是一些其他的信息，因此这个假设可能会限制模型的效果。  
contributions:1.作者对于直接操作于图结构数据的网络模型根据频谱图卷积(Hammond等人于2011年提出的)使用一阶近似简化计算的方法，提出了一种简单有效的层式传播方法。    
2.作者验证了图结构神经网络模型可用于快速可扩展式的处理图数据中节点半监督分类问题，作者通过在一些公有数据集上验证了自己的方法的效率和准确率能够媲美现有的顶级半监督方法    
method:在这项工作中，我们直接使用神经网络模型f（X，A）对图结构进行编码，并在带有标签的所有节点的有监督目标L0上进行训练，从而避免损失函数中基于图的正则化。首先定义一个具有以下分层传播规则的多层图卷积网络（GCN）：   
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/2.jpg)     
2.1 谱图卷积：   
由于作者的方法是基于频谱图形卷积进行改进提出的，因此先介绍频谱图形卷积，基于图形数据操作的频谱卷积被定义为信号输入量x与经过傅里叶域参数化的滤波器gθ=diag（θ）进行相乘操作，其运算公式如下所示：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/3.jpg)    
式中U代表归一化图形拉普拉斯算子L=IN-D-1/2AD-1/2=UΛUT的特征向量的矩阵，Λ是它的特征值对角矩阵，UTx是x的图形傅里叶变换，作者提出将gθ理解为为L特征值的一个函数即gθ(Λ)，由于频谱卷积所提出的公式（3）计算开销过于庞大，特征向量矩阵U的相乘运算时间复杂度是N的平方，此外，L的特征分解运算开销在处理数据量大的图结构时也很大，为了解决这个问题，2011年Hammond等人提出gθ(Λ)可以通过一个切比雪夫多项式Tk(x)的截断展开为第K阶：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/4.jpg)  
式中，λmax表示L的最大特征值，θ'是一个切比雪夫系数的向量，切比雪夫多项式是被递归的定义为Tk(x)=2xTk-1(x)-Tk-2(x)且T0(x)=1并且T1(x)=x；将上述操作代入到式(3)中则可得：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/5.jpg)    
式中L运算过程与(3)中Λ一致，这个表达式现在是K的范围内了。因为gθ被约等于拉普拉斯算子中的一个K阶多项式了，运算过程只和卷积位置的节点K步（边长）范围内的点有关，式(4)的计算复杂度则变成了O(|ε|)
2.2层状线性模型  
接着上述的工作，作者提出了层状线性模型，首先，作者将每层的卷积运算按式(5)来操作并将K设为1，这样就得到了在图形化拉普拉斯频谱上的一个线性函数，借助这种线性运算方式，作者仍然可以通过堆叠多个这样的卷积层运算来恢复卷积滤波函数的表达能力，而且作者摆脱了受限于切比雪夫不等式进行显式化参数设置的限制。作者直观的觉得他的这个模型能够通过较宽的节点分布来减轻分类问题中节点邻近区域结构的过拟合问题，此外。从运算开销来看，将K设置为1得到的层状线性运算也能够进一步构建更深的网络模型（例如运用何凯明提出残差结构构筑更深的图形化卷积网络模型）  
在上面提到的将K设为1的图形化卷积的线性运算基础上，由于作者期望神经网络模型中的参数能够适应训练过程中的变化，作者进一步近似的令λmax约等于2，式(4)在这个改变下进一步简化为：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/6.jpg)   
式中有两个自定义系数Θ’0和Θ’1，由于是卷积神经网络，滤波器参数对整幅图操作时遵循权值共享原则，连续地运用这种滤波器操作能够搞笑的卷积各个节点的k阶（k阶指的是第k层卷积层）相邻区域。通常来说，限制参数的数量可以进一步解决过拟合并将各层的运算量最小化，因此，基于这个思想可进一步将Θ’0和-Θ’1都等价于θ简化式（5）得到下面的表达式:  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/7.jpg)   
式中IN+D-1/2AD-1/2计算过程使得特征值处于[0,2]之间，在不断地进行D-1/2AD-1/2D-1/2AD-1/2网络层叠加操作的过程中这样的运算可能会导致数值不稳定产生梯度爆炸/消失的结果，为了减轻这个问题，作者进一步引进再归一的技巧：，其中，且，借助该改进作者将该定义泛化到一个有C个输入通道的输入量（或者图数据中每个节点都是C维的特征向量）与F个滤波器运算定义如下：  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/8.jpg)   
式中θ是滤波器参数的矩阵形式，Z是卷积后得到的输出矩阵.
3.半监督节点分类问题：  
在下文中，我们考虑在具有对称邻接矩阵(二进制或加权)的图上半监督节点分类的两层GCN，前向模型采用简单的形式:  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/10.jpg)   
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/9.jpg)   
左图：用于半监督学习的多层图卷积网络（GCN）的示意图，在输出层中具有C输入通道和F特征图。 图形结构（边缘显示为黑线）在各层之间共享，标签由Yi表示  
右图：t-SNE（Maaten＆Hinton，2008）使用5％的标签在Cora数据集上训练的两层GCN的隐层激活可视化（Sen et al。，2008）。 颜色表示文档类别。  
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/GNN-methods/SEMI-SUPERVISEDCLASSIFICATION%20WITHGRAPHCONVOLUTIONALNETWORKS/11.jpg)  
