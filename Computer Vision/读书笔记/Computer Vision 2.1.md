#### 2.1 几何元操作和转换

在这一张接种，我们主要介绍2d 和3d 的“元”，它们是点线面。我们也会介绍3d 的图像是如何投影成为2d 的。

##### 2.1.1 几何元

集合元构成了描述三维图形的基本块 basic building blocks。这一章节我们会介绍点线面，后面会讨论曲线，表面以及体积

##### 2D 的点

2D 的点（在图像中的像素坐标）可以用一对值表示 
$$
x=(x,y)\in R^2
$$
或者
$$
\begin{bmatrix}
x \\
y
\end{bmatrix}
$$

$$
我们用(x_1,x_2,...)来表示列向量
$$

2D 的点同事可以用齐次坐标 homogeneous coordinates 来表示

##### 齐次坐标系：

欧式空间采用 (x,y,z)来表示一个三维点，但是无穷远点 (∞,∞,∞)在欧式空间里是没有意义的，在投影空间中进行图形和几何运算并不是一个简单的问题，为了解决这个问题，数学家 August Ferdinand Möbius提出了齐次坐标系，采用 N+1N+1 个量来表示 NN 维坐标。例如，在二维齐次坐标系中，我们引入一个量 w，将一个二维点 (x,y)表示为 (X,Y,w)的形式，其转换关系是
$$
x = \frac{X}w \\
y = \frac{Y}w
$$
例如，在欧式坐标中的一个二维点 (1,2)可以在齐次坐标中表示为 (1,2,1)(1,2,1)，如果点逐渐移动向无穷远处，其欧式坐标变为 (∞,∞)，而齐次坐标变为 (1,2,0)，注意到在齐次坐标下不需要 ∞ 就可以表示无限远处的点。

##### 2D 线

2D 的线也可以用齐次坐标系来表示，
$$
\tilde{l} = (a,b,c)
$$
它对应的 line equation 是
$$
\bar{x}\cdot\tilde{l} = ax+by+c=0
$$
 We can normalize the line equation vector so that l = (^nx; ^ny; d) = (^n; d)  with k^nk = 1 . In

this case, ^n  is the normal vector  perpendicular to the line and d  is its distance to the origin

(Figure 2.2 ). (The one exception to this normalization is the line at infinity ~l = (0; 0; 1) ,

which includes all (ideal) points at infinity.)

2D 中的线段l可以表示为（n，d），n 是一个垂直于 l 的向量，d 是 l 到原点的距离

3D 中的平面 m 可以表示为（n，d），n 也是一个垂直于平面 m 的向量，d 是平面 m 到原点的距离![wed22nov1132](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/wed22nov1132.png)

上面提到的 n 还可以用极坐标 ploar coordinates 来表示，也就是一个Θ角度就可以了

如果使用的是 homogeneous coordinates，可以通过叉乘 cross product 来计算交点：
$$
\tilde{x} = \tilde{l}_1 \times \tilde{l}_2
$$
x 表示叉乘，也可以通过两个点叉乘得到两点确定的直线：
$$
\tilde{l} = \tilde{x}_1 \times \tilde{x}_2
$$


