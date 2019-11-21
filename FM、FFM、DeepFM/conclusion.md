注：对FM、FFM、DeepFM系列做整理与总结  
引：在传统的线性模型中，每个特征都是独立的，如果需要考虑特征与特征之间的相互作用，可能需要人工对特征进行交叉组合。非线性SVM可以对特征进行核变换，但是在特征高度稀疏的情况下，并不能很好的进行学习。现在有很多分解模型可以学习到特征之间的交互隐藏关系，基本上每个模型都只适用于特定的输入和场景。推荐系统是一个高度系数的数据场景，由此产生了FM系列算法。
参考文章：https://blog.csdn.net/hiwallace/article/details/81333604   
https://www.jianshu.com/p/6f1c2643d31b  
https://www.jianshu.com/p/152ae633fb00  
https://www.jianshu.com/p/781cde3d5f3d  
1.FM算法（Factorization Machines）
FM(Factorization Machine)主要是为了解决数据稀疏的情况下，特征怎样组合的问题。已一个广告分类的问题为例，根据用户与广告位的一些特征，来预测用户是否会点击广告。数据如下：
![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/1.png)  
clicked是分类值，表明用户有没有点击该广告。1表示点击，0表示未点击。而country,day,ad_type则是对应的特征。对于这种categorical特征，一般都是进行one-hot编码处理。将上面的数据进行one-hot编码以后，就变成了下面这样 ：  ![Image text](https://github.com/wqf321/recommandation-reading/blob/master/FM%E3%80%81FFM%E3%80%81DeepFM/2.png)  
