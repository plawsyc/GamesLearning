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
    * **四元数** 待补充——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————

## 7、Viewing Transformation（观测变换）
### **7.1 Model Transformation(模型变换)**
### **7.2 View/Camera Transformation(视图变换)**
* **定义相机**

    position：相机的**位置向量**![](http://latex.codecogs.com/gif.latex?\vec{e})

    Look At Direction：相机的**朝向**![](http://latex.codecogs.com/gif.latex?\hat{g})

    Up Direction：相机的**法向量**![](http://latex.codecogs.com/gif.latex?\hat{t})(与![](http://latex.codecogs.com/gif.latex?\hat{g})正交)

    <img src="image/视图变换1.png" width=300px>

* **将物体坐标变换到相机视角**

    想象相机和物体都处在世界坐标系下，相机和物体的位置都是依据世界坐标系而言的。

    我们现在要做的事情就是将物体的位置用相机的视角描述，这本质上是**世界坐标系**到**相机坐标系**（相机为原点，![](http://latex.codecogs.com/gif.latex?Y)轴为![](http://latex.codecogs.com/gif.latex?\hat{t})，![](http://latex.codecogs.com/gif.latex?Z)轴为![](http://latex.codecogs.com/gif.latex?-\hat{g})，![](http://latex.codecogs.com/gif.latex?X)轴为![](http://latex.codecogs.com/gif.latex?Y\times{Z})）的坐标变换。

    <img src="image/视图变换4.png" width=500px>

    由于相机位置与坐标原点并不重合，无法直接使用基变换，因此首先要将相机与物体同时移动![](http://latex.codecogs.com/gif.latex?-\vec{e})，

    <img src="image/视图变换2.png" width=400px>

    然后应用基变换矩阵：

    <img src="image/视图变换3.png" width=500px>

    > 注：基变换相关知识可参考《线性变换的本质》，设物体在相机坐标系下的向量坐标为![](http://latex.codecogs.com/gif.latex?\vec{c}), 在世界坐标系下向量坐标为![](http://latex.codecogs.com/gif.latex?\vec{w})，那么存在基变换![](http://latex.codecogs.com/gif.latex?V)，能够将向量在相机坐标系中的描述转变为世界坐标系下的描述，即![](http://latex.codecogs.com/gif.latex?V\vec{c}=\vec{w})，那么显然，![](http://latex.codecogs.com/gif.latex?V)的列向量由![](http://latex.codecogs.com/gif.latex?\hat{g}\times{\hat{t}})、![](http://latex.codecogs.com/gif.latex?\hat{t})、![](http://latex.codecogs.com/gif.latex?-\hat{g})组成。而我们要求的从世界坐标系到相机坐标系的基变换矩阵![](http://latex.codecogs.com/gif.latex?R_{view})应满足![](http://latex.codecogs.com/gif.latex?\vec{c}=R_{view}\vec{w})，所以显然![](http://latex.codecogs.com/gif.latex?R_{view}=V^{-1}=V^{T})(这一步的基变换只涉及旋转，而前面讲过，旋转矩阵是正交的)。

### **7.3 Projection Transformation(投影变换)**：3D->2D
<img src="image/投影变换1.png" width=500px>

* **正交投影(Orthographic)**

    通过[l,r],[b,t],[f,n]六个值定义了一个空间中的长方体，然后将它投影到一个中心位于原点的![](http://latex.codecogs.com/gif.latex?[-1,1]^3)的正方体上，

    <img src="image/正交投影.png" width=500px>

    > 注1：摄像机沿 **-Z** 方向朝物体看去，因此f(far)相比n(near)要离得原点近；

    > 注2：这也正好解释了之前的视图变换中摄像机的LookAt向量为何对应 **-Z** 方向，这是因为在 **右手系** 中，朝 **-Z** 方向看去看到的正好是XOY坐标系，取得物体的正交投影只要去掉物体投影变换后的Z坐标就可以了。也正因如此，OpenGL等图形API中使用的是**左手系**。

    > 注3：注意理解投影变换与视图变换的不同作用，上图中空间中的长方体就是摄像机能够呈相的全部区域，因此正好要投影到![](http://latex.codecogs.com/gif.latex?[-1,1]^3)的正方体上。(存疑)

* **透视投影(Perspective)**

   为了方便理解，我们将透视投影分为两步，一是**挤压**/**裁剪**，变换用矩阵![](http://latex.codecogs.com/gif.latex?M_{persp->ortho})表示，二是正交投影，变换用![](http://latex.codecogs.com/gif.latex?M_{ortho})来表示。

    <img src="image/透视投影1.png" width=500px>

   * **挤压**

        挤压是将由近平面![](http://latex.codecogs.com/gif.latex?n)与远平面![](http://latex.codecogs.com/gif.latex?f)包围成的楔形体挤压成长方体的过程。

        **近平面上的点坐标保持不变，远平面上的点的![](http://latex.codecogs.com/gif.latex?z)坐标保持不变。远平面的中心点保持不变。**

        <img src="image/透视投影2.png" width=500px>

        通过图片看到，远平面上点挤压后的![](http://latex.codecogs.com/gif.latex?x)与![](http://latex.codecogs.com/gif.latex?y)坐标与原坐标的比例关系可通过相似三角形得到。
        
        从而从近平面到远平面上任意一点的坐标经过挤压有如下变换：
        
        <img src="image/透视投影3.png" width=500px>

        > 注：为什么要变换后的坐标要乘![](http://latex.codecogs.com/gif.latex?z)，这是为了使之后的投影变换矩阵中不出现变量，从而方便计算机的存储和运算。

        根据变换的前后坐标，容易写出不完全的变换矩阵：

        <img src="image/透视投影4.png" width=500px>

        > 注：最后一行为什么是![](http://latex.codecogs.com/gif.latex?[0,0,1,0]),而不是![](http://latex.codecogs.com/gif.latex?[0,0,0,z])，这与上一个注中的理由相同，就是投影矩阵中不应该出现变量。而这一步之所以能这样表示也是因为在上一步中的齐次坐标乘了![](http://latex.codecogs.com/gif.latex?z)。

        现在只需要求投影矩阵的第三行：依旧通过能够确定的点坐标进行反推：

        对于近平面上的点，有：![](http://latex.codecogs.com/gif.latex?z=n)，所以可以确定其变换前后的坐标：

        <img src="image/透视投影5.png" width=300px>

        变换后的![](http://latex.codecogs.com/gif.latex?z)坐标为![](http://latex.codecogs.com/gif.latex?n^2)，与![](http://latex.codecogs.com/gif.latex?x)、![](http://latex.codecogs.com/gif.latex?y)无关，那么其对应的挤压矩阵的第三行的前两个数一定是0，那么还只需求![](http://latex.codecogs.com/gif.latex?A)和![](http://latex.codecogs.com/gif.latex?B)：

        <img src="image/透视投影6.png" width=500px>

        这时需要另外一个约束条件，远平面上的点的![](http://latex.codecogs.com/gif.latex?z)坐标也不变，从而有：

        <img src="image/透视投影7.png" width=500px>

        从而解出：

        <img src="image/透视投影8.png" width=400px>

        将其带回![](http://latex.codecogs.com/gif.latex?M_{persp->ortho})挤压矩阵，再乘以正交投影矩阵就可以得到透视投影矩阵：

    
        <img src="https://latex.codecogs.com/svg.image?M_{persp->ortho}=\begin{pmatrix}n&0&0&0\\0&n&0&0\\0&0&n&plus;f&-nf\\0&0&1&0\\\end{pmatrix}" width=400px> .

        <img src="image/透视投影9.png" width=300px> 
        
        > 注：显然，**透视投影变换不是仿射变换**，因为它不能通过线性变换+平移得到。

        > **透视投影中只有近平面与远平面的![](http://latex.codecogs.com/gif.latex?z)坐标不变，位于二者之间平面上的点的![](http://latex.codecogs.com/gif.latex?z)坐标应该怎样变换呢？**

        >对于近平面与远平面中任意一点![](http://latex.codecogs.com/gif.latex?[x,y,z,1])，对其应用挤压变换有：\
        <img src="https://latex.codecogs.com/svg.image?\begin{pmatrix}n&0&0&0\\0&n&0&0\\0&0&n&plus;f&-nf\\0&0&1&0\\\end{pmatrix}\begin{pmatrix}x\\y\\z\\1\end{pmatrix}=\begin{pmatrix}nx\\ny\\(n&plus;f)z-nf\\z\end{pmatrix}==\begin{pmatrix}nx/z\\ny/z\\n&plus;f-nf/z\\1\end{pmatrix}" />

        >将其第三项与原![](http://latex.codecogs.com/gif.latex?z)坐标对比，<img src="https://latex.codecogs.com/svg.image?n&plus;f-nf/z-z=-(z-f)(z-n)/z>0" title="n+f-nf/z-z=-(z-f)(z-n)/z>0" />，所以经过挤压的中间点相比原来要更加靠近近平面(在右手系中，相机朝向为![](http://latex.codecogs.com/gif.latex?-\vec{z})轴，所以点的![](http://latex.codecogs.com/gif.latex?z)坐标越大距离相机越近)。

        >事实上，这一结果也符合“挤压”的形象理解，想象一下，保持小端不变，挤压一块楔形海绵的大端，是不是除了横向位移固定的大端和小端平面，内部所有的点都有向小端横向移动的倾向？
# **Leture 05: Rasterization（Triangles）**
## 8、透视投影补充：范围定义
* 透视投影中定义锥形体范围：**视角(FOV)** 和 **宽高比(aspect ratio)** 
    
    前文中我们通过近平面与远平面只定义了![](http://latex.codecogs.com/gif.latex?z)方向的投影范围，![](http://latex.codecogs.com/gif.latex?x)与![](http://latex.codecogs.com/gif.latex?y)方向上的投影应该怎么定义呢？

    与正交投影中直接定义![](http://latex.codecogs.com/gif.latex?[l,r])与![](http://latex.codecogs.com/gif.latex?[b,t])平面不同，透视投影的范围要靠摄像机的 **视角（Field of View，FOV）** 与 **屏幕的宽高比（(aspect ratio）** 二者定义。

    <img src="image/透视锥体1.png" width=400px>

    > 视角分为横向视角fovX与竖向视角fovY，这两个视角中的任一个与宽高比组合都能完整描述一个透视锥体。这里使用的是fovY。

    假定透视远平面根据![](http://latex.codecogs.com/gif.latex?z)轴中心对称，![](http://latex.codecogs.com/gif.latex?y_{max}=t)，![](http://latex.codecogs.com/gif.latex?x_{max}=r)。那么fovY与aspect ratio可以按下图计算：

    <img src="image/透视锥体2.png" width=600px>


## 9、光栅化
### **9.1 屏幕与光栅**

* 图形学中的理想屏幕：
    * 一个像素点的二维数组
    * 二维数组的大小：分辨率（resolution）
    * 典型的光栅成像设备
* 光栅化的含义
    * 德语中光栅（Raster）就是屏幕（screen）的意思
    * 光栅化（Rasterize）就是把东西画在屏幕上
* 像素（Pixel）的意义
    * 是picture element的缩写
    * 认为像素是成像的最小方块
    * 暂时认为一个像素内部颜色一致（当然事实不一定是这样）
* 屏幕空间定义
    * 这里假定屏幕原点放置在左下角
    <img src="image/屏幕空间.png" width=600px>

### **9.2 视口变换（Viewport transform matrix）**

我们之前经过一系列变换将物体投影到了![](http://latex.codecogs.com/gif.latex?[-1,1]^3)的正方体上，现在我们则要将这个正方体转换到屏幕上，这一变换就是**视口变换**：

首先，暂时丢弃正方体的![](http://latex.codecogs.com/gif.latex?z)坐标不考虑。

这样就是将![](http://latex.codecogs.com/gif.latex?[-1,1]^2)的平面投影到![](http://latex.codecogs.com/gif.latex?[0,width]\times{[0,height]})的屏幕上，容易写出对应矩阵（先缩放后平移）：

<img src="image/视口变换.png" width=400px>

### **9.3 显示设备**
* 阴极射线管（Cathode Ray Tube，CRT）：示波器等早期显示设备
    * 逐行扫描显示
    * 隔行扫描显示（基于人的视觉暂留效应，一帧奇数行，一帧偶数行，现在有时用于视频压缩）

    <img src="image/CRT.png" width=300px>

* 直接将显存中一块区域的数据对应到屏幕上
* 液晶显示器（Liquid Crystal Display，LCD） 
    利用偏振光的波动特性，例如下图中光先通过右侧的竖向光栅，波动方向变为竖直，若是图下方的情况在通过横向光栅时会全部被挡下来，而在图上方的光通过中间的一系列液晶对光的振动方向进行了偏转，因此能够通过左侧横向光栅，从而被看到。

    <img src="image/LCD.png" width=300px>
* LED（Light emitting diode，发光二极管） 发光二极管阵列组成的屏幕
* 电子墨水屏：Kindle（Electronic Ink Display）
    通电墨水上浮，断电墨水下降：
    <img src="image/电子墨水屏.png" width=300px>
### **9.4 Triangles**

* 为什么三角形作为图形学的基本图元？
    * 三角形是最基础的多边形，并且能组合成任意复杂多边形
    * 三角形的三个顶点一定在同一个平面上
    * 判断一个点是否在三角形的内部十分方便（叉积应用）
    * 已知三个顶点的属性，容易对三角形内部的点属性进行插值。

* 三角形光栅化

    * **采样**屏幕上的像素点，判断它是否在三角形内部。

        <img src="image/像素采样.png" width=400px>

        ```
        for(int x=0; x < xmax; x++){
            for(int y=0; y < ymax; y++){
                PixelArray[x][y] = IsInside(Triangle, x+0.5, y+0.5);//像素位于三角形则置1，否则置0.
            }
        }
        ```

    * 如何判断点是否在三角形内部

        向量叉积

        > 注：如果点在两个三角形的边界上怎么算？自行定义即可。

    * 优化方法：AABB轴向包围盒，Bounding Box

        因为这样一个长方形的包围盒的范围由三角形三个顶点中最小和最大的x，y坐标框定，所以包围盒刚好容纳了三角形可能覆盖的所有区域。

        这样就只需要对包围盒内部（图中蓝色部分）的像素点进行IsInside的判定，节省了运算时间。

        <img src="image/光栅化中的包围盒.png" width=400px>
    
    * 优化方法：Incremental Triangle Traversal

        对三角形每一行的最左端到最右端像素逐行计算。
        
        <img src="image/光栅化的优化2.png" width=400px>

        这适用于某些包围盒优化不明显的三角形，例如瘦长的或倾斜占包围盒比例过小的情况：

        <img src="image/特殊情况.png" width=400px>

    * 现实世界中的像素点：每个像素都是由R，G，B不同的颜色点组成的（每个颜色的都是光通过不同颜色的滤镜呈现的）：

    <img src="image/实际像素点.png" width=600px>

    还可以看到三星Galaxy的屏幕中绿的点要比红蓝多，这是因为人眼对绿色更为敏感，这种排列图案称为Bayer Pattern。
# 番外1：sRGB、Linear色彩空间/Gamma矫正/线性工作流
## 问题：为何存在sRGB空间？
* 人眼对亮度的感知是非线性的，对暗部感觉更敏感而对亮部感觉更不敏感，如下图：

    人眼认为的均匀渐变：<img src="image/人眼.png" width=600px>

    物理线性的均匀渐变：<img src="image/线性.png" width=600px>

* 颜色采样及存储是离散的，如果均匀存储线性颜色，我们将使用相同数量的单元存储对人眼来说信息较多的暗部和人眼感觉不敏感的亮部，这不但会带来存储上的浪费，还使得当用来表示颜色的比特有限时，在显示时会出现较明显的色阶断层：
    
    想象一下，当想描述一个类似上图1的对人眼较为舒服的渐变时，右侧物理亮部只占大约1/4，而需要一般的存储单元，这使得亮部采样过于密集而暗部采样过于稀疏，当采样频率不高时，人眼在分辨不出亮部的断层时却能发现暗部出现了色阶断层。
* 总结：原因1：人眼对亮度感受非线性；原因2：颜色的离散性存储。

## Gamma矫正：sRGB空间与线性空间的转换
* 鉴于以上原因，我们在存储颜色是执行的是sRGB非线性标准，这意味着我们需要将物理世界的真实亮度转换到sRGB格式，这就是Gamma变换：![](http://latex.codecogs.com/gif.latex?L_{sRGB}=L_{linear}^\gamma)。
同样的，当颜色显示出来时，需要一次相反的Gamma变换，将sRGB空间变换到线性空间。对于前者![](http://latex.codecogs.com/gif.latex?\gamma=1/2.2<1),指数曲线是上凸，因此称为Gamma上调，对于后者![](http://latex.codecogs.com/gif.latex?\gamma=2.2>1),指数曲线下压，因此成为Gamma下压。并且，后者往往由显示器设备自动完成。不难看到，经过Gamma上调的图片，如不进行下压，整体颜色将会更亮。

## 线性工作流
* 当渲染器读进一张sRGB图片时，如果直接在其上进行光照计算（光照是线性的），计算之后经过Gamma下压输出到屏幕上，原颜色得以正常显示，而光照额外经过了一次下压，这导致，当光照亮度小于1时，色彩变暗，而光照亮度大于1时，色彩变强，表现出来就是场景整体偏暗，同时对比度增强。见下图：

    Gamma空间下计算光照：<img src="image/Gamma空间.png" width=600px>

* 而如果渲染器在接受sRGB图片后，先将其转换到线性空间，然后进行线性的光照计算，最后将计算结果经过Gamma上调后输出，并最终经过显示器的Gamma下压显示到屏幕上，才是正确的光照结果：见下图：

    线性空间下计算光照：<img src="image/线性空间.png" width=600px>

* 可以看出，线性空间下计算结果对比度减弱了，颜色的过度也较为平滑，本质上是光照较弱的部分得到了增强，而高光部位有所减弱。线性空间下的光照得到了正确的计算，可以有效的解决一些场景过暗或者过亮的问题，在PBR中能更真实地模拟现实世界，但是这不意味这Gamma空间下的计算一定要被舍弃，在一些风格化渲染或者对比度要求较为强烈的场合，Gamma空间仍有其用武之地。

## Unity3D下的线性工作流
* Unity默认的颜色空间是Gamma空间，在这一模式下是否勾选纹理的sRGB选项没有影响，因为Unity都不会对其做任何转换，在计算结束后Unity也不会进行任何Gamma矫正。
* Unity的线性空间需要在Project/setting中切换，切换之后，若纹理的sRGB选项被勾选，则Unity载入纹理时会先Gamma下压将其转换到线性空间，光照计算之后会进行Gamma上调后再写入颜色缓(或者线性写入后再Gamma矫正，这取决于当前渲染配置(ref:《UnityShader入门精要》))。而如果sRGB选项未被勾选，则Unity载入纹理时不会对其进行Gamma变换，而是将其视为线性空间下的图片。




