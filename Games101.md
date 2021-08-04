# Games101笔记
## 1、图形学中一般向量表示为列向量
## 2、当一个坐标系的坐标轴存在![](http://latex.codecogs.com/svg.latex?x\times&space;y=z)，则称该坐标系为右手坐标系，注意课程中默认坐标系为右手系，而OpenGL等图形API中默认为左手坐标系，也即![](http://latex.codecogs.com/svg.latex?x\times&space;y=-z)的坐标系.
## 3、向量叉乘的应用：
* 判断左右，例如，在右手系的XOY平面上，若两个向量![](http://latex.codecogs.com/gif.latex?\vec{a})，![](http://latex.codecogs.com/gif.latex?\vec{b})有![](http://latex.codecogs.com/gif.latex?\vec{a}\times&space;\vec{b})的结果的![](http://latex.codecogs.com/gif.latex?z)分量为正，则![](http://latex.codecogs.com/gif.latex?\vec{b})在![](http://latex.codecogs.com/gif.latex?\vec{a})左边，否则在右边。

   <img src="image\叉乘应用1.png" width="400px"/>
* 进而，可以利用上述性质判断一个点是否在三角形的内部（用于光栅化）

   例如，对于逆时针描述的三角形ABC，若AP在AB左侧，BP在BC左侧，CP在CA左侧，则P在ABC内部，反之若任意AP、BP、CP在对应边的右侧，则认为其在三角形外。

  当然若按顺时针A-C-B的顺序构造三角形，则需满足AP，CP，BP分别在AC，CB，BA的右侧。
  所以，判断一个点是否在三角形内侧，可以通过判断它与顶点构成的向量是否均在三条对应边的同侧（逆时针为左侧，顺时针为右侧）来实现。

  <img src="image\叉乘应用2.png" width="200px"/>
## 4、2D矩阵变换与齐次变换
>注：该部分内容结合《线性代数的本质》可深入理解
* 结合《线性代数的本质》中对线性变换的定义：

    变换后的坐标系原点位置不变并且“网格线”平行且等距分布，将2D变换分为**线性变换**：**旋转**，**缩放**，**剪切**以及**线性变换**：**平移**（坐标原点发生了改变）
 ### **4.1 线性变换**：旋转（rotate） 缩放（scale） 剪切（shear）
* 直接将变换后的$x$轴与$y$轴基向量作为变换矩阵的两个列向量即得到变换矩阵：
	（方法见《线性代数的本质》）
	
    **旋转**：

    <img src="image\旋转矩阵.png" width="200px"/>

    **缩放**：

    <img src="image\缩放矩阵.png" width="200px"/>

    **剪切**：

    <img src="image\剪切矩阵.png" width="200px"/>
### **4.2 平移变换**：升维——>引入第三个维度：

<img src="image\平移变换1.png" width="200px">



 <img src="image\平移变换2.png" width="300px"/>
