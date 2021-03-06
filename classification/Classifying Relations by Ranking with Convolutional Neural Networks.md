#### note:
　　本文是关系抽取任务中的分类模型，给定句子“周星驰执导了《美人鱼》电影”和待确定pair（周星驰，美人鱼），确定给出的几组关系中其正确的关系类型（假定此处为“导演”）。整体思路是对句子编码最后进行分类：
  1. 获取句子中每个词的embedding表示r；
  2. word position embedding。position embedding——由句子中当前词与pair词的相对位置信息拼接得到wpe，再由r和wpe拼接得到word position embedding（句子中每个词都有）；
  3. 使用CNN-max得到最终的句向量；
  4. 添加全连接层，最终维度为类别大小；
  5. 使用rank loss计算损失。

#### comment:
　　文章出彩的地方在于pair词信息的引入和loss设计。loss着重考虑了当前样本最可能被错分的类别，假定正确类别是A，但模型预测“排除A”后，最大概率出现C，则反馈时，会尽量拉开A和C的距离，使当前样本预测为A的概率上升，预测为C的概率下降，这种方式仅考虑极端的情况，在多分类任务中会有比较好的效果。不足之处是，模型类别之间是等价的，假定上例中模型预测为B（A与B从类别上更相近），在反馈时会以同样的梯度去更新参数，而不会减小对B的惩罚力度。

#### more:
  [tensorflow code](https://github.com/pratapbhanu/CRCNN)
