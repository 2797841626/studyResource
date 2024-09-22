##### 项目背景
由于robomaster赛场上对于视觉的要求越来越高，队伍不得不考虑如何击打旋转中的装甲板，再解决如何击打旋转装甲板之前需要解决的最大问题便是对于装甲板姿态的估计，openCV中solvepnp虽然可以求解姿态，但其精度显然以不能满足要求，因为识别没办法保证角点能非常准确，以致pnp对旋转矩阵的求解非常困难。但在rm赛场上，装甲板的安装需要按照官方规则手册，这便使得装甲板的pitch角和roll角固定，所以我们可以将这两个值固定，将原本六个自由度的方程降为四个自由度，使得求解更加准确。
##### 坐标系
在构建方程前，我们先得了解世界坐标如何投影到图像上的。这里我们定义三个坐标系，分别为世界坐标系，相机坐标系和图像坐标系。
###### 世界坐标系
以我们主观意志确定的原点的三维坐标系，例如我们可以将世界坐标系原点设置在装甲板中心，如下图，这样便可知道灯条四个角点的世界坐标。
![[3.jpg]]
###### 相机坐标系
相机坐标系，顾名思义就是以相机光心为原点建立的坐标系（画画技术一般，莫怪）
![[4.jpg]]
###### 图像坐标系
也称作像素坐标系，比如一张图是（1280，1024），1280便是这张图的宽，1024便是这张图的长，里面任何一点都可由像素点去表示。

##### 现在我们来讨论这几个坐标系之间的转换
###### 世界坐标系转相机坐标系
![[5.jpg]]
如图，我们将世界坐标系定在了装甲板中心，z轴方向为垂直装甲板方向，如何将这个坐标系转换成相机坐标系呢，这里需要注意到，任何两个坐标系之间的转换都是经过**旋转**和**平移**得到的。
此处讲个题外话，经常有人会困扰到底是先平移还是先旋转？其实可以先旋转后平移，也可以先平移后旋转，但根据先后顺序不同，平移矩阵会发生改变，因为平移矩阵需要根据确定的坐标系给出，比如你的平移矩阵可以是在世界坐标系下的平移矩阵，也可以是相机坐标系下的矩阵。如果你想从世界坐标系转到相机坐标系，此刻若想先平移后旋转，那么你的平移矩阵肯定是通过世界坐标系给出。但要是先旋转后平移呢，那就变成通过相机坐标系给出了。
我们这采取先旋转后平移的方式
此处我不讲解旋转矩阵如何求，可用高中几何知识一个一个旋转去求，先转roll，再转pitch，再转yaw便可将世界坐标系的方向转成相机坐标系的方向了，再进行平移便可重合
设世界坐标系为

$$
\begin{bmatrix}
Xw \\
Yw \\
Zw \\
\end{bmatrix}
$$

设相机坐标系为
$$
\begin{bmatrix}
Xc \\
Yc \\
Zc \\
\end{bmatrix}
$$  

则
$$
R*\begin{bmatrix}                  
Xw \\
Yw \\
Zw \\
\end{bmatrix}+T =  
\begin{bmatrix}
Xc \\
Yc \\
Zc \\
\end{bmatrix} $$

其中R为旋转矩阵，T为平移矩阵

R = Ry * Rx * Rz
$$
Ry = \begin{bmatrix}
cos(yaw) & -sin(yaw)  & 0 \\
      sin(yaw)  & cos(yaw)  & 0 \\
         0     &     0    &   1  \\
\end{bmatrix},
Rx = \begin{bmatrix}
cos(pitch)  & 0  & sin(pitch) \\
          0  &      1    &   0  \\    
     -sin(pitch)  & 0  & cos(pitch) \\
\end{bmatrix},
Rz = \begin{bmatrix}
1    &   0    &       0      \\
      0   &  cos(roll) &  -sin(roll) \\
      0 &   sin(roll)  &  cos(roll) \\
\end{bmatrix}
$$
     
    
###### 相机坐标系转图像坐标系
我这里直接用网上非常经典的图来展示相机坐标如何映射到图像坐标的。（主要是自己真画不出来！！！）
![[6.png]]
P为相机坐标下的点，p’为映射到图像上的点，他们之间的关系可以用相似三角形确定。
假设一个像素表示d毫米，焦距为f毫米，图像坐标为（x,y）,相机坐标为（Xc,Yc,Zc）
则有x * d / Xc = f / Zc,同理y * d / Yc = f  / Zc。
即x = (f/d)  * (Xc/Zc), y = (f/d)  * (Yc/Zc).
又由于图像坐标系的原点是在左上角，所以需要分别加上图像中心坐标u0，v0得到最终的图像坐标系为u' =  (f/d)  * (Xc/Zc) + u0,  v' =  (f/d)  * (Yc/Zc)+v0.
用矩阵表示为
$$
Zc\begin{bmatrix}                  
u' \\
v' \\
1 \\
\end{bmatrix} =  
\begin{bmatrix}
f/d & 0 & u0 \\
0 & f/d & v0\\
0 & 0 &1 \\
\end{bmatrix} \begin{bmatrix}                  
Xc \\
Yc \\
Zc \\
\end{bmatrix}$$
###### 世界坐标系转图像坐标系
相机坐标系转图像坐标系与前面的世界坐标系转换到相机坐标系相结合，便可得到世界坐标系转换成图像坐标系的公式。
$$
Zc\begin{bmatrix}                  
u' \\
v' \\
1 \\
\end{bmatrix} =  
\begin{bmatrix}
f/d & 0 & u0 \\
0 & f/d & v0\\
0 & 0 &1 \\
\end{bmatrix}  *[R*\begin{bmatrix}                  
Xw \\
Yw \\
Zw \\
\end{bmatrix}+T]$$
##### PNP求解
仔细思考会发现，世界坐标已知，相机内参通过标定已知（f/d,u0,v0）,图像坐标已知，R与T即是我们希望求到的。可如何求呢。
首先装甲板的长宽已知，就代表四个角点的世界坐标（Xw,Yw,Zw）已知，其次，根据视觉识别，我们也已知图像坐标u‘，v’。机智的人已经发现，这岂不是就是解这个方程即可。但现实情况是我们这个方程因为误差的原因多半是无解的，我们只能找到近似解。
如何寻找近似解？学过优化算法的人这个时候就会想到用最小二乘来建立模型，然后用牛顿迭代来计算极值便是近似解了。
我们的世界坐标通过一系列转换变成了图像坐标u‘,v'，与图像中的识别坐标u,v按理来说应该重合，所以我们建立的模型便是
(此处u'是由世界坐标映射回图像坐标的横坐标，u为识别得到的角点的横坐标，v'与v为纵坐标)
 $$ Min（u'-u）^2 + (v'-v)^2$$
又因为有四个点所以最终的模型为
此处
 $$F(a,b,c,roll,pitch,yaw) =  Min 
∑（u'-u）^2 + ∑(v'-v)^2$$

u’是关于a,b,c,roll,pitch,yaw的函数，所以求解该极值的方法便是求函数F(a,b,c,roll,pitch,yaw)的Jacobi矩阵和Hessina矩阵进行牛顿迭代。
然后迭代方式为

$$
\begin{bmatrix}                  
a \\
b \\
c \\
roll \\
pitch\\
yaw \\
\end{bmatrix} =  
\begin{bmatrix}                  
a \\
b \\
c \\
roll \\
pitch\\
yaw \\
\end{bmatrix}  - Jacobi/Hessian$$

根据robomaster比赛场上的特殊情况，可以降低两个自由度，将pitch固定为15°，将roll固定为0°，于是公式变为：
$$
Zc\begin{bmatrix}                  
u' \\
v' \\
1 \\
\end{bmatrix} =  
\begin{bmatrix}
f/d & 0 & u0 \\
0 & f/d & v0\\
0 & 0 &1 \\
\end{bmatrix}  *[R*\begin{bmatrix}                  
Xw \\
Yw \\
Zw \\
\end{bmatrix}+T]
 $$
 $$
其中  R=\begin{bmatrix}
cos(yaw) & -sin(yaw)  & 0 \\
      sin(yaw)  & cos(yaw)  & 0 \\
         0     &     0    &   1  \\
\end{bmatrix}*
\begin{bmatrix}
cos(15)  & 0  & sin(15) \\
          0  &      1    &   0  \\    
     -sin(15)  & 0  & cos(15) \\
\end{bmatrix}
,T = \begin{bmatrix}                  
a \\
b \\
c \\
\end{bmatrix}
 $$

将矩阵表达式展开成数学表达式：
$u'(a,b,c,yaw) = f/d*(Xw*cos(yaw) - Yw*sin(15)*sin(yaw) + a)/(-Xw*sin(yaw)-Yw*sin(15)*cos(yaw) + c) - u0$
$v'(a,b,c,yaw) = f/d*(Yw*cos(15) + b)/(-Xw*sin(yaw)-Yw*sin(15)*cos(yaw) + c) - v0$

继续用最小二乘法建模：
 $$ Min F（a,b,c,yaw） =  Min
∑（u'-u）^2 + ∑(v'-v)^2$$
ps : 优化过程中注意避免除0，一般我会选择将除法转换成乘法，读者可以开动脑筋想如何修改该模型使其没有除法运算。
此时u'，v'只是关于（a,b,c,yaw）的函数，
我们对F（a,b,c,yaw）求Jacobi矩阵和Hessian矩阵。个人建议不要手算，因为比较复杂，可借助python求导库辅助求导，以免求导出错。我电脑环境不全，本可以用ceres的求导库一步到位，由于比较懒，没有配置ceres库。
这是用python求偏导与二阶导，可想其复杂性。
![[Pasted image 20240412114215.png]]
迭代方式为
$$
\begin{bmatrix}                  
a \\
b \\
c \\

yaw \\
\end{bmatrix} =  
\begin{bmatrix}                  
a \\
b \\
c \\

yaw \\
\end{bmatrix}  - Jacobi/Hessian$$
至此，重写pnp就结束了。

##### 效果展示
这是识别到的装甲板
![[Pasted image 20240412113649.png]]

相机是平放在地面处
左边装甲板求出来的姿态为
![[Pasted image 20240412113849.png]]
右边装甲板求出来的姿态为
![[Pasted image 20240412113925.png]]
均符合预期。