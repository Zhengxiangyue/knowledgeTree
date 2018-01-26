block huffman

average bitrate of huffman

bitrate , compression ratio

p[a|b] 给定 b 的情况下 a 的概率\

P[0/1] = P[10] / P[11]

000101 中 00出现的概率： 两对00，一共五对



transform 

decorelate，

energy compaction ， 

important and not important dataquantize differently



scalar / vector quantize



transform matrix 的特征

walsh

fft and dct  优点和缺点

复数-》向量 和 x 的角度

向量的长度和角度哪一个，视觉敏感



两张图 fft，一张图片的像素向量长度换成另一张图像的，向量角度不变

ifft 以后更像角度一样的图片



隐藏水印





bitrate 比特率 = bits per symble = number of bits in the coded bitstream / bumber of samples

Compression Ratio = size of the uncompressed signal / size of the coded bitstream

压缩前的大小比压缩后的大小

SNR，信噪比的计算方法



#### 1.介绍

-  什么是压缩
   -  用更少的表示方式来表示数据


-  压缩的目标
   -  极大程度减少数据大小来满足存储/带宽要求


-  压缩的限制
   -  重建要保证无损或有损但可用（无损压缩和有损压缩）


-  压缩的策略
   -  减少冗余
   -  利用人类视觉不敏感的特点



#### 3.压缩/编码 中的基本定义

-  coding：compression
-  码字codeword：一个用来表示整个编码后数据或单个 symbol 的二进制串
-  coded bitstream：用来表示整个编码后的数据的二进制串
-  无损压缩：百分百准确恢复原数据
-  有损压缩：对数据的 reconstruction 会带来一些可以容忍的错误
-  比特率：Average number of bits per original data element after compression

$$
bitrate = \frac {number\ of\ bits\ in\ the\ coded\ bitstream}{number\ of \ samples\ (or\ pixels)}
$$

-  Compression Ratio(CR)

-  $$
   Compression\ Ratio = \frac{size\ of\ the\ uncompressed\ signal}{size\ of\ the\ coded\ bitstream}
   $$




-  比特率是压缩之后的 bit 数比上原数据的 symbol 数目
-  压缩比是压缩前的数据大小比上压缩后的数据大小



-  Signal to Noise Ratio(SNR) 在有损压缩中才有意义：

   -  假设输入信号为I = {x1,x2,x3...xN}, 压缩后重建的信号为R = {y1,y2,y3...yN}

   那么信噪比计算公式等于
   $$
   SNR = 10\log_{10} {\frac{y_1^2+y_2^2+...+y_N^2}{(x_1-y_1)^2 + (x_2-y_2)^2 + ... + (x_N-y_N)^2}}
   $$
   范数符号：
   $$
   ||E||^2=x_1^2+x_2^2+x_3^2+...x_N^2
   $$
   均方差(Mean-Square Error)
   $$
   MSE = (x_1-y_1)^2+(x_2-y_2)^2+(x_3-y_3)^2 + ...(x_N-y_N)^2
   $$
   Relative Mean-Square Error
   $$
   RMSE = \frac {||I-R||^2}{||I||^2}
   $$
   ​

   ## Lossless compression

   #### 3.GOLOMB CODING

   golomb 编码是一种切实有效的方法，它是一种 Run-Length 对二进制串编码的实践


-  golobm coding of order m, 首先需要计算一个编码的值 m，最优值是距离 p*ln(2)/(1-p)这个值最近的一个2的幂，p 是0和1之中出现概率较高的值的概率，所以要先根据输出流中1和0出现的频率计算出 m
-  ​