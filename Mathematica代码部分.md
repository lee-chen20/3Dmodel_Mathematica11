#### 几何参数

```mathematica
(*unit:   millimeters*)
d0 = 15.6; h0 = 4.5;     (*云台凸部*)
d1 = 8.8; h1 = 5.6;
d2 = d0; h2 = 3;
H = h0 + h1 + h2 + 30.7;(*总高度*)   DoH = 25.5;(*云台凸部以上*)
D0 = 17.7; H0 = 13.4;(*主轴凹部*) 
ZoD0 = H - DoH/2;(*凹部中心坐标 (0,0,ZoD0)*)
doD0 = (DoH - D0)/2;(*凹部外圈厚度*)
yoS = -D0/2;(*螺丝y坐标*)
doS = 5;(*螺丝孔直径*)
dRoS = 4;(*螺丝孔外厚度*)
H4 = H - 1.2;(*螺丝孔入口处高度*)
coS = 1.3;(*缺口高度*)
LoS = 22;(*螺丝孔深*)
Lon = 5.2;(*螺母厚度*)
don = 8.1;(*螺母宽度*)
```

#### `RegionPlot3D`建模

```mathematica
modle = RegionPlot3D[   ((x^2 + y^2 <= (d0/2)^2 && 
       h0 + h1 + h2 + 
         2 - \[Sqrt]((2 + h0 + h1 + h2 + 2)^2 - x^2 - y^2) <= z <= 
        h0) || (x^2 + y^2 <= (d1/2)^2 && 
       0 <= z - h0 <= h1) || (x^2 + y^2 <= (d0/2)^2 && 
       0 <= z - h0 - h1 <= h2)(*云台凸部*)
     || (x^2 + y^2 <= (DoH/2 - 1)^2 && 
       0 <= z - (h0 + h1 + h2) <= 
        1) || ((x^2 + y^2 <= (DoH/2)^2 && 
          h0 + h1 + h2 + 1 <= z <= 
           Sqrt[(DoH/2)^2 - x^2 - y^2] + ZoD0(*云台凸部以上 大块*)
           || (x^2 + (y - yoS)^2 <= (doS/2 + dRoS)^2 && 
           h0 + h1 + h2 <= z <= H4   )(*螺丝孔壁*)  ) && 
       y^2 + (z - ZoD0)^2 >= (D0/2)^2 (*挖去主轴凹部*) &&  ! (Abs[x] > 
           H0/2 && z - (ZoD0 - 1.5) >= -Sqrt[(DoH/2)^2 - 
              y^2])(*挖去凹部两侧*)&& ! (x^2 + (y - yoS)^2 <= (doS/2 + 
              dRoS)^2 && z > H4)  )  )
   && ! (  
     x^2 + (y - yoS)^2 < (doS/2)^2 && 
      z > H4 - LoS - Lon - 1.3)(*挖去螺丝孔内*)
   && ! (Abs[z - (H4 - LoS/2)] < coS/2 && y < 0)(*挖走C型 缺口*)
   && ! (y - (yoS + don/Sqrt[3]) < -x/Sqrt[3] && 
      y - (yoS + don/Sqrt[3]) < x/Sqrt[3] && Abs[x] < don/2 && 
      H4 - LoS - Lon < z < H4 - LoS)(*挖走螺母孔内*)
  
  
  , {x, -20, 20}, {y, -20, 20}, {z, -5, 55},
  PlotPoints -> 300,(*精度*)
  PlotStyle -> Directive[LightBlue, Opacity[0.3]], Mesh -> None, 
  ImageSize -> Medium, BoxRatios -> {40, 40, 60}]
```

#### 建模文件输出为.stl，可打印，Windows10可直接打开预览 

```mathematica
Printout3D[modle, "D:\\model3D.stl", TargetUnits -> "Millimeters"]
```

