# Games101笔记
# **Leture01~Leture03**
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

    变换后的坐标系原点位置不变并且“网格线”平行且等距分布，将2D变换分为**线性变换**：**旋转**，**缩放**，**剪切**以及**非线性变换**：**平移**（坐标原点发生了改变），线性变换与平移加起来都叫做**仿射变换**。
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
	
这里第三个坐标只对点有用（为1），对向量没用（为0），因为向量具有平移不变性.

<img src="image\平移变换2.png" width="400px"/>

>**注：对于一个平移与线性变换组合的矩阵，其表示的是先线性变换后平移变换：** 
> <img src="image\先线性后平移.png" width="300px"/>

* 形象理解：

    上述变换实际是将向量空间由二维坐标映射到了三维空间，然后做了一个X，Y轴不变(或线性变换)，使Z轴向XOY平面靠近/远离的剪切操作：

    <img src="image/升维剪切.png" width="450px"/>

    	
	对应point也跟随坐标系的位置发生了剪切变化：![](http://latex.codecogs.com/gif.latex?z)坐标仍为1，![](http://latex.codecogs.com/gif.latex?x)坐标与![](http://latex.codecogs.com/gif.latex?y)坐标跟随基向量分别改变了![](http://latex.codecogs.com/gif.latex?t_x)与![](http://latex.codecogs.com/gif.latex?t_y) 。此时其横坐标与纵坐标就分别是![](http://latex.codecogs.com/gif.latex?x+t_x) 与![](http://latex.codecogs.com/gif.latex?y+t_y) 。
* 为什么第三个坐标对point是1，对vector是0？：

   <img src="image/vec&point1.png" width="200px"/>

    容易看出，这很方便定义上述的三种操作：0+0=0；1-1=0；1+0=1

    那么point+point的结果1+1=2如何解读呢？对这一操作有：

    <img src="image/point+point.png" width="300px"/>

    因此point+point得到的结果就是这两个点的中点。

# **Leture 04: Transfromation cont.**
## 5、正交矩阵特性解读与旋转矩阵

>正交矩阵的特性：\
1、**矩阵每列向量与自己点乘为1，与其他列向量之间点乘为0。**\
2、对于正交矩阵![](http://latex.codecogs.com/gif.latex?M),其与转置矩阵相乘![](http://latex.codecogs.com/gif.latex?MM^T)对角线上元素为列向量自乘，非对角线元素为列向量互乘，因此![](http://latex.codecogs.com/gif.latex?MM^T=E)。也即：**正交矩阵的逆矩阵与转置矩阵相同**。

将正交矩阵看作应用于任一列向量上的线性变换，则基于上述**特性1**，变换的后的向量空间的基向量满足1）单位向量、2）相互正交。因此，显然**单一的旋转变换是正交变换**，同样容易想到，**镜像变换（将某一基向量方向取反）也是正交变换**。

正交矩阵的**特性2**可应用于旋转矩阵的逆变换——已知某个旋转矩阵![](http://latex.codecogs.com/gif.latex?R_\theta), 其对应的逆旋转变换与转置矩阵相同：![](http://latex.codecogs.com/gif.latex?R_{-\theta}=R_\theta^T)。

<img src="image/正交矩阵与旋转变换.png" width="300px">

## 6、三维变换与对应齐次坐标

三维变换与二维变换的齐次坐标一样，在点或者向量后面再加一位，1（末尾有效）代表点，0（末尾无效）代表向量。\
<img src="image/三维变换1.png" width="350px"> \
与之对应的，三维变换对应的变换矩阵也就是4*4的矩阵：\
<img src="image/三维变换2.png" width="400px">

* **缩放**\
<img src="image/缩放.png" width="300px">

* **平移**\
<img src="image/平移.png" width="300px">

* **旋转**\
<img src="image/旋转.png" width="600px">\
显然，三维空间中绕某一坐标轴的旋转矩阵，是该坐标轴作为基向量不变，另外两个基向量在对应基平面内进行二维旋转所构成的。
    > 注：**绕Y轴旋转后得到的基向量与绕其他轴得到的基向量互逆，这是因为X轴与Z轴构成的基平面是ZOX平面而非XOZ平面，实际的旋转空间中坐标轴的顺序与4*4矩阵中坐标轴的排列顺序相反。**

* **复合旋转**

    * **欧拉角（Euler angles）**

        <img src="image/复合旋转1.png" width="300px"/>

        **Pitch（抬头低头）、Roll（沿轴翻滚）、Yaw（左右旋转）**:

        <img src="image/欧拉角.png" width="300px"/>

    * **欧拉角（Euler angles）**    
    * **罗德里格斯旋转公式（Rodrigues' Rotation Formula）**

        <img src="image/罗德里格斯旋转公式.png" width="600px"/>

        该公式表示了围绕 **经过坐标原点的轴![](http://latex.codecogs.com/gif.latex?\vec{n})** 旋转![](http://latex.codecogs.com/gif.latex?\alpha)角度的旋转矩阵。

        >注：如果想绕不经过坐标原点的轴旋转，则应该先将旋转轴平移变换到坐标原点处，然后应用罗德里格斯旋转矩阵，然后再平移回原来的位置。

        推导：

        <img src="image/推导1.jpg" width="600px"/>

        <img src="image/推导2.jpg" width="600px"/>

        >注：注意体会推导Step1过程中向量点积与矩阵变换对偶性的应用；以及公式最后一项N为向量叉积的矩阵表示形式。
