-  JEPG提供了一种压缩”连续色调“、彩色和单色图的工具箱
-  基本的 JPEG 提供了一种基于 DCT 的算法，并应用 run-length 编码以及哈夫曼编码
-  基本的 JPEG 只能在”sequential mode“ 下实现，并且智能对8bit/pixel 的图像进行操作
-  扩展的 JPEG 提供了多种可算则的enhancements：
   -  12-bit/pixel 的输入图像
   -  progressive transmission 渐进式传输
   -  可在 arithmetic 和哈夫曼编码之间进行选择
   -  适应性量化 adaptive quantization
   -  tiling平铺
   -  静态图片交换文件格式（SPIFF， still picture interchange file format）
   -  选择性修复/改进（selective refinement）
   -  JPEG的应用
      -  Desktop publishing, color fax, photojournalism, medical images, general image archiving systems, consumer imaging, graphic arts, and others
      -  桌面出版，彩色传真，新闻摄影，医学图像，一般图像存档系统，消费者图像，图形艺术等



#### The Baseline JPEG Algorighm

1. 它对图像的每个8x8的 blocks 进行操作
2. mean-normalization 均值规范化
3. 变换：对每个 block 进行 DCT 变换
4. 量化
   -  用户需要提供一个8x8的量化矩阵Q
   -  每一个 block 除以 Q(point by point)
   -  每个值近似到最接近的整数
   -  注意：每个图像最多可以使用4个量化矩阵（一个luminance亮度，三种颜色每个一个）
5. 对 DC 系数进行熵编码（每个量化后的 block 的最左上角的系数），使用 DPCM+哈夫曼编码
   -  对 DC 的”残余数据“哈夫曼编码，”残余数据“是这样来的：是没一个 DC term 和前一个 DCterm 的差值（DPCM)
6. 对 AC 系数进行熵编码（AC 是除了 DC 的所有项）
   ![DCAC](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/DCAC.jpeg)
   -  用 Z 字形排列 AC terms
   -  记录每一个非零系数的值（称为 level）以及它到上一个非零系数的距离（称为 run）
   -  对[run,level]进行哈夫曼编码（后面介绍详细步骤）



#### The Quantization Matrices

-  他们是由用户提供的

-  可以通过对比度敏感度函数HVS 来计算

-  矩阵中的每个数时8位整数

-  它们通过按比例缩放一个常量来控制比特率（They provide control over the bitrate by scaling them by a constant factor

-  一个矩阵的例子：Example of Q:

   | 16   | 11   | 10   | 16   | 24   | 40   | 51   | 61   |
   | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | 12   | 12   | 14   | 19   | 26   | 58   | 60   | 55   |
   | 14   | 13   | 16   | 24   | 40   | 57   | 69   | 56   |
   | 14   | 17   | 22   | 29   | 51   | 87   | 80   | 62   |
   | 18   | 22   | 37   | 56   | 68   | 109  | 103  | 77   |
   | 24   | 35   | 55   | 64   | 81   | 104  | 113  | 92   |
   | 49   | 64   | 78   | 87   | 103  | 121  | 120  | 101  |
   | 72   | 92   | 95   | 98   | 112  | 100  | 103  | 99   |



#### 对 DC 的残余矩阵进行编码

1. DC **残余矩阵**的范围时[-2047,2047]

2. 每一个DC 的值的绝对值在0到2047 = 2^11 - 1之间

3. 把这个范围分成12个子范围 subranges 或叫做 categoried（0 ~11）。category 0 的范围是 **2^(k-1)** 到 **2^k - 1**(category 0 中只有一个整数0)

4. 假设 r 是任意一个 DC residual. |r| = s^(k-1) + t, 0 <= t <= 2^(k-1) - 1,并且 t 可以用 k-1 位二进制码来表示

   >  k       r
   >
   >  0       0
   >
   >  1	 1
   >
   >  2	 2 3
   >
   >  3	 4 5 6 7
   >
   >  ...
   >
   >  11	 512 … 1022 1023

   如上表，t 就是序列中的一个位置，比如 r=6，对应 k=3，6 = 2^(3-2) + 2，所以 t 就是2，用二进制表示为10

5. 这样 r 就可以用 s,k,t 表示，其中 s 是 r 的符号（注意前面提到的 r 的范围）

6. 为这12个 category 建立一个哈夫曼树（也就是为不同的 k，0~11建立哈夫曼树），每一个 codeword 最多16位长
   问题：对于一张给定的图像，怎样计算这12个 category 的概率
   答：可以计算出全部 DC 的 category，然后计算每个 category 有多少个，就可以计算出频率，用频率代替概率
   **问题：如何修改哈夫曼生成算法使得哈夫曼树的高度最高为16？或者一个给定的高度 m？**
   **答：当树的高度达到给定值时，不再继续增加，而是把新的节点作为它的cousin 节点？？？**

7. 每个 DC residual 中的一个 r 可以用二进制代码 **hsm** 来表示

   -  其中 h 是 r 所在 category k 用哈夫曼编码后的codeword
   -  s 是 r 的符号，0代表负数，1代表正数
   -  m 是用 k-1位表示的 t



#### 对 AC terms进行编码

最后会把所有(d,x)变成一个可以用哈夫曼编码的形式

1. 所有非零 AC terms 的绝对值都小于2^10 - 1

2. 对于一个非零的 AC term 称他为 x，d 是 x 到前一个非零 AC term的距离（他们俩中间0的个数），我们最后需要表示所有的（d,x）

3. 1 <= |x| <= 2^10 - 1

4. 把 x 的范围分成是个自范围，和 DC 类似。category 是0到9。

5. 用 s（x 的符号位）和它的绝对值|x|来表示，s=0表示小于零，1表示大于零

6. 可以用 k 和 t 来表示|x| = 2^(k-1) + t， 其中 k可以用 k-1 位来表示

7.  t can be represented in binary using k-1 bits.

8. x可以用 s，k，t 来表示

9. （d,x）可以用（d,k,s,t）来表示

10. d在0到62之间（在一个块中，如果只有最后一个 AC 非零，那么这时 d 就是62，这种情况好像不怎么可能出现）

11. 把 d 写成这种形式： **d = 15p + r**, r 的范围是0,1,2...14

12. r 可以用四位表示
    $$
    r_3r_2r_1r_0
    $$
    由于只表示0到14，它不可能时1111，其他值都有可能

13. 把 p 用这种形式表示：
    $$
    11110000_111110000_211110000_3...11110000_p
    $$

14. 于是 d 可以表示成：
    $$
    11110000_111110000_211110000_3...11110000_pr_3r_2r_1r_0
    $$

15. category k 的范围是 1到10（讲义上这么写的）可以用四位二进制来表示
    $$
    k_3k_2k_1k_0
    $$

16. 这里要把 d 和 x 中的 k 放在一起表示：
    (d,x) => (d,k,s,t)，其中的(d,k)可以表示成：
    $$
    11110000_111110000_211110000_3...11110000_pr_3r_2r_1r_0k_3k_2k_1k_0
    $$

17. 以上这个（d,k）可以用 p+1字节来表示

18. 因为 r 是0到14，15个数，k 有10个可能的数，所以最后一个字节有15*10种不同的可能

19. 再加上11110000这个特殊字符和一个 end-of-block(EOB)符号（EOB 代表一个8x8的块中的 ACterms 结束了）

20. 一共有152个不同的符号来建立哈夫曼树

21. Build a Huffman table for those 152 symbols, where every codeword is at most 16 bits long 

22. JPEG encodes each quantized AC term (d,x)=(d,k,s,t) as 

    hsm

     where

    -  h is the Huffman codeword of (d,k)
    -  s= sign of the term; s=0 if negative, 1 if positive
    -  m= the (k-1)-bit binary representation of t.

#### MPEG(1&2):基础概念

-  ​