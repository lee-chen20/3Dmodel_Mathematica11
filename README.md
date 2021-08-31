# 3Dmodel_Mathematica11

使用三脚架拍照期间，不慎摔了一下，导致云台下的旋转枢纽碎裂。手头常用的工具是Mathematica11，查询后发现支持3D建模，故尝试**用Mathematica 11来做云台枢纽的3D建模**。

### 目标

从胶水黏起来的云台枢纽，测量参数、3D建模后进行3D打印（可找厂商完成打印），成品可以完美替代碎裂部件。

### 条件

Mathematica 11两个内置函数：

+ `RegionPlot3D`

  可以画出满足给定条件（例如不等式）的三维几何体（点集的抽样）

+ `Printout3D`

  输出为指定的3D模型文件

### 思路

将云台枢纽部件解构为一系列简单几何体，球、圆柱等等（参数需要测量），可使用不等式表达。

重建云台枢纽，就是简单几何体之间内嵌或结合等，也就是**众多不等式代表的点集求交集、并集或补集**。

理论上难度不大。

### 实现
源码[model3D.nb](https://github.com/lee-chen20/3Dmodel_Mathematica11/blob/main/model3D.nb)需用版本号不低于11的Mathematica打开，内部包含了视觉效果良好的3D图像。另外提供了.nb文件的[pdf易读版](https://github.com/lee-chen20/3Dmodel_Mathematica11/blob/main/model3D.pdf)，以及仅包含代码的[Markdown文件](https://github.com/lee-chen20/3Dmodel_Mathematica11/blob/main/Mathematica%E4%BB%A3%E7%A0%81%E9%83%A8%E5%88%86.md)。

3D建模结果输出为[model3D.stl](https://github.com/lee-chen20/3Dmodel_Mathematica11/blob/main/model3D.stl)，可直接打印，也可用Windows10直接预览。



打印出来的成品符合要求，三脚架满血复活（撒花）。

缺憾之处在于，`RegionPlot3D`函数对复杂点集取样，似乎不能很好的区分界面和内部，以至于想要实现更好的精度就必须算得更多、模型存储文件也会更大。
