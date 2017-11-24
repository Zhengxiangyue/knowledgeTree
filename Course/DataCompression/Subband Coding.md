Subband Coding

1.动机

-  DCT 变换的问题
   -  区块失真，特别是在低比特率
   -  降低失真的方法，例如 overlapped transforms，代价高而且复杂
   -  对整个图片进行 DCT，而不是对一小块 block，会忽略图片不同区域的频率差别（？）ignores the significant differences in frequency contents in various regions of the image, thus leading to less quality-bitrate performance

-  wavelets/subband coding 的优点
   -  对整个图像进行操作而不是分成块
   -  可以避免 blocking artifacts 效应
   -  在图片的各个区域动态调整空间/频率的方案While dynamically adjusting the spatial/frequency resolution to the appropriate level in various regions of the image


-  在实际操作中，wagelet/subband 编码的效果旗鼓相当，有的时候甚至更好，特别是在低比特率的时候



2.线性滤镜 Linear Filters

-  线性滤镜的定义
   -  一个线性滤镜 f 由一串实数表示，
   -  给输入信号x = xn 加滤镜的过程是这样的:

![filter](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/filter.gif)

也就是离散的卷积形式：

![convelution](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/convelution.gif)

如果 X，Y，F 表示 x，y，f 的傅里叶变换

那么 Y = FX

时域卷积等于频域相乘



3.低通滤镜

-  一个低通滤镜会消除高频内容并且保留下低频内容

-  一个理想的低通滤镜f的傅里叶变换 F在[0，a)的范围内是一个非零常数，在[a,]之间是0

   ![LPF](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/LPF.jpg)

   应用：降噪以及图像 smoothing



4.高通滤镜

-  高通滤镜会消除低频信号，保留高频信号
-  理想的高通滤镜 f 的傅里叶变换 F 是这样的![HPF](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/HPF.jpg)


-  应用：sharpening，edge detection
-  理想的高通滤镜和低通滤镜在实际使用中是不可靠的，but many realizable filters are good approximations of ideal filters
-  如果一个滤镜有有限个数字，它叫做 finite-impulse-response 滤镜，否则叫做 infinite-impulse response 滤镜