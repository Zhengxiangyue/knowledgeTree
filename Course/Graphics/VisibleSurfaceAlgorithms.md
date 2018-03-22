什么是 Visible Surface Determination

给很多个3D 物体，然后给定相机位置和姿态，要确定哪些直线或者面是 visible 的

也叫做 hidden surface removal

如果只考虑线的遮挡关系，叫做 visible-line determination

注意：线本身不遮挡线

##### Image Precision

对于每一个像素点（假设一共有 N 个像素点），要判断哪一个物体（假设有 M 个物体）是可见的

对于每一个像素，离相机最近的并且在这个像素上有显示的物体，就应该显示它的颜色

##### Object Precision

把每一个物体都直接和其他的物体进行比较：

#### 高效判断可见面判别算法：

利用斜率，

透视变换

后向面剪切

Extents and bounding volume 包围体，把复杂的 object 包在简单的封闭空间里面

空间分割：分解问题称为更小的问题：例如把屏幕分割成更小的 grid 格子，在这个小格子里面解决问题，

Hierarchy等级制度：Build tree of nested extents
If two internal nodes don’t intersect, none of their children
intersect 

#### Coherence

- Object coherence
  如果两个 object 在 x-y 坐标系中是完全不相交的，在计算一个 object 的时候就没有必要考虑另一个 object
- Face coherence
   面的深度可以根据移动的多少以及斜率来确定
- Edge coherence
   斜率
- Implied edge coherence



Z-BUFFER 算法

3D 深度 sort

BSP



