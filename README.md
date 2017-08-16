
对贝叶斯从指标上如何构造模型
朴素贝叶斯属于生成模型，即通过类别的的先验概率，和特征在类别下的条件概率，得到同时发生的概率。不涉及参数，只是概率的生成。
为什么可以这样：
我们回想贝叶斯的原理可以知道贝叶斯的最大特点就是不把参数看成固定的值，而是先验概率来确定可能性。
这也是和概率模型最大的区别。到朴素贝叶斯这里，我只要学习一套规则，规则可以区分就行。因为规则太多了人直接想不太可能。因此要给模型数据，让计算机去学
那么规则怎么定义。
我想分类垃圾邮件和非垃圾邮件，那么垃圾邮件之所以是垃圾邮件是因为他里面有很多垃圾词汇，而这些垃圾词汇组成一张表，
那么就用贝叶斯来搞定了，p（x,y)代表xy同时发生的概率，那么除以x发生的概率这里x可以由全概率计算。这样其实就是一个比重。
实际操作中有以下问题：
同时发生概率怎么计算，存储的单词应该是有意义的单词，不然矩阵就太大了。因此需要一张停词表。
然后各种概率就用最大似然估计，然后就有一个问题如果出现了如果出现了未曾遇到词怎么办，这就需要赋值了，具体赋值多少还要看结果来挑。
最后就是判断垃圾邮件非飞垃圾邮件。我们的目标是保证正式邮件不被弄错，因此我们可以提高垃圾比重，垃圾非垃圾之比是个大概率才判定。
至于多少是大概率还要自己判定。
具体实施细节：
需要另个dict来保存正常和垃圾邮件所包含的信息。假如两个都存在可以利用贝叶斯来判断，假如一个存在另一个不存在，需要赋值。作完这些后就得到了
一个条件概率，即某个词的出现意味着多大概率的垃圾邮件。那么这么多元素怎么处理，
那么一篇文章所有的有用信息都拿进来，涵盖掉大部分特征的文件必然是垃圾邮件。因此同时发生的概率，是表明涵盖大部分信息的。坏的发生概率，与总的概率之比重就是需要的。所以为了不误判，提高阈值。
