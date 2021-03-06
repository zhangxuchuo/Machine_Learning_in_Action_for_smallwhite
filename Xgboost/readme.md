#### XGBoost学习方法
如果有大佬看到代码内容有问题并愿意指正，请及时联系cethuang@gmail.com，诚挚感谢指导

要学习代码，建议先看如下内容！！！

v1.2 20190722

- XGBoost的代码实现主要分为以下步骤

1. 代码主要使用的损失函数为最小二乘损失，这个损失函数的使用简单，理解方便，不清楚的可以在网上自行搜索资料学习透彻后再进行下一步
2. 代码主要计算了最小二乘损失的梯度和海赛函数，梯度实际上是函数的一阶导数，海赛实际上是函数的二阶导数，是一阶导函数的导数。这样组成的函数可以构建二阶泰勒公式，泰勒公式主要用于定义一个新的更方便求解极值的函数用于近似原函数。
3. XGBoost的相比GBDT更大的进步不仅仅在于使用了二阶泰勒公式（GBDT使用的是梯度），能够更好的拟合损失函数。另外一个大的特点是在损失函数中加入了正则化项，实际上，正则化项常见于逻辑斯蒂回归、SVM、神经网络等，这些正则化项主要是对参数θ进行正则化。而XGBoost的正则化主要是对叶子节点进行正则化，这实际上是希望对叶子节点进行惩罚，如果叶子节点生成过多，那么可以通过正则化项对叶子节点这一项的总值进行缩小，那么实际上就起到了剪枝的作用；同时XGBoost还对决策树的分裂次数进行正则化，这实际上是希望分裂出来的决策树相对简单，那么可以更好的解决对未知数据预测能力的过拟合问题。根据《统计学习方法》决策树相关章节的论述，决策树模型的损失函数同样也加入了正则化项目，但是决策树仅仅对叶节点加入了正则化，并未对决策树的分裂次数进行正则化。
4. 本代码实现为了简单，方便理解，在所有的公式中，都没有加入正则化项，也就是令λ=0，w=0。至于不明白正则化作用的，建议学习的内容是吴恩达的网易云课堂的机器学习正则化相关视频，可以清晰的理解正则化项到底对损失函数做了什么。
5. 本代码基于GBDT迭代生成，因此相关的测试数据均可以在本仓库下的GBDT下学习后再进行，强烈不建议没有掌握GBDT相关知识的同学直接上来学习XGBoost。
6. 正确的学习路径应该是决策树-CART回归树-Boosting-AdaBoost-GBT-GBDT-XGBoost-LightGBM。XGBoost相关的列采样方案建议还需要学习补充随机森林相关算法知识。决策树到GBT相关知识非常建议学习《统计学习方法》相关章节，内容翔实，案例简单。
7. XGBoost算法的核心代码主要是基于面向对象设计，对super超类继承等方法不熟悉的同学还需要补充面向对象相关方法，方可以理解本代码实现。本代码实现注释了非常多的内容，虽然繁琐，但是方便初学者理解。
8. 学习本代码可以先结合本仓库GBDT下的GBDT_XGBoost_LGBM算法原理v1.1.PPT对XGBoost和相关公式推导有一个概念理解，然后再进行代码阅读。
9. 本代码中定义的Gain增益系数，和决策树的Gini基尼系数是两个概念，请勿混淆，至于具体的XGBoost的Gain增益值是如何计算并打分的，请参照第8点的材料进行阅读。


v1.1 20190715

- 添加知乎上一张图说明XGBoost算法的图例，可以结合wepon的算法材料学习



v1.0 20190713

- 本方法仅建议机器学习初学者参考，大佬求放过
- 对于初学者不要看到各种Kaggle比赛、腾讯广告算法大神的大佬用了LGBM（XGBoost变种）等XGBoost算法就想着经典机器学习算法不想看了先学习最牛逼的算法的思想
- 所有牛逼的算法都有一个牛逼的爹。XGBoost的很牛逼是因为有多个牛逼的爹。列举部分如下：决策树爹、正则爹、泰勒公式爹、随机森林爹（列采样基因）、排序爹（Boosting为何能够做到并行）、梯度下降爹……
- 你应该先去看李航《统计学习方法》中关于CART决策树的基础理论，掌握一颗决策树是如何遍历所有切分点然后找到最佳切分点的
- 你应该先去学习梯度下降算法，至少应该看懂一元函数和多元函数求偏导数的方法、如此你还需要去复习一下极限理论
- 你应该先去学习决策树的损失函数，了解决策树的损失函数是如何控制分裂点
- 你应该先去学习正则化的思想，强烈建议去看网易云课堂的吴恩达机器学习关于正则化的相关解释，非常适合我这种白痴，至少应该懂得，惩罚项到底在干嘛，我一开始看到惩罚项，以为是前面的损失函数做错了什么事情，例如私下河塘洗澡之类的需要受到惩罚，损失函数到底做错了什么。正则化L1和L2两种，至少应该理解经典的等高线图的交叉点概念
- 先学习Boosting算法。掌握加法模型和Bagging算法的不同之处在哪儿，比如和随机森林的区别点在什么地方
- 然后去掌握提升树算法，理解提升树在拟合数据的时候使用残差的概念，实际上就是在求定义为平方损失函数的梯度。
- 然后去掌握梯度提升树算法，理解为何需要升级成这个算法，在面临什么场景的时候，提升树算法就不灵了，需要使用到梯度来求解类似SoftMax这样的损失函数
- 然后你理解了上面的GBDT，你可以开始尝试去学习XGBoost，别急，看到蛋疼的原论文的时候很痛苦，找一下知乎，找一下github的可能是东半球最大的学习组织（罗永浩？）等地方，慢慢的啃，一步步的来，才是适合我这样的白痴学习方法。
