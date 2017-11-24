向量量化

1.背景以及动机

-  标量量化对像素之间的相互关系是不敏感的
-  标量量化不仅不能利用像素之间的关系，它还会 destroy 这些关系，从而损坏照片的质量
-  所以，对相互之间有关联的数据进行量化，需要另外一种量化级数，来最大限度的保留数据之间的correlation
-  Vector quantization(VQ) 就是这样一种技术
-  向量量化是标量量化的推广：它对 vectors（连续的数据块）进行量化，而不是单一的像素
-  向量量化可以用作单独的压缩技术来对原数据直接进行压缩
-  VQ 也可以在一些有损压缩中的量化阶段使用，特别是在 transform 阶段没有完全破坏相互关系的情况下，像有些 wavelet 变换的应用

2.VQ 的主要技术

-  建立一个字典或者“视觉词汇表”，叫做“codebook”，里面的单元是“codevectors”，每一个“codevector”都是一个一维或者二维的 block
-  Coding 编码
   -  1.将输入数据分割成 blocks（vectors），每个 vector 有 n 个像素
   -  2.对于每个 vector u，在 codebook 字典中找到一个与之最为匹配的 codevector，假设它是 u‘，用 u’ 的 index 代替 u
   -  3.对量化后的 indices 进行无损压缩
-  Decoding 解码（简单的查找 table）
   -  1.对压缩后数据解压缩得到的 indices（量化后的数据）
   -  2.用 i 对应的 codevector代替索引 i



-  codebook 必须要被保存或是传输
-  The codebook can be generated on an image-per-image basis or a class-per-class basis



3.VQ 的一些问题

-  codebook 的 size，即有多少个 codevectors
-  codevector 的size
-  codebook 的建立：应该包含哪些 codevector？
-  codebook 的结构：为了最快的最好的匹配搜索
-  Global or local codebooks: class- or image-oriented VQ?



4.codebook 的大小和 codevector 的大小（权衡）

-  如果 codebook 的 size Nc 比较大，他可以代表更多的特性，重建以后的质量比较好
-  但是如果 Nc很大，需要更多的空间去存储或传输
-  比较小的 Nc 就相反，重建以后的质量没有那么好但是需要的空间比较小
-  比较典型的 Nc 的值会是：2^7,2^8,2^9,2^10,2^11
-  如果 codevector 的 size 比较大，可以更好的利用像素之间的相互关系
-  但n不应该大于空间相关的程度



5.codevector 的 size 最优化

