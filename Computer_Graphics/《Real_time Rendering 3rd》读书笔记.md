# 渲染管线

## 功能

- 在给定虚拟相机，物体，光源，照明模式以及纹理等诸多条件下，生成一幅二维图像

- 渲染管线是实时渲染的底层工具

![img](《Real_time Rendering 3rd》读书笔记.assets/2FB977C1E4AAACE29B1DCEB426B97795.png)

## 三个阶段

- 应用程序阶段

  - 主要任务：将需要在屏幕上显示绘制出来的图元（点线面）输入到下一阶段
  - 通常用于实现碰撞检测，加速算法，输入检测，动画，力反馈，纹理动画，变换仿真，几何变形等方法。

- 几何阶段

  - 主要任务：多边形和顶点的操作

  - 细分

    ![img](《Real_time Rendering 3rd》读书笔记.assets/95CFB22E39982F4A61827C0F1EBA1652-16423918417491.png)

    - 模型视点变换
      - 目的：便于投影和裁剪
      - 效果：相机放在坐标原点，前-z，上y，右x，得到相机空间，模型变换到适合渲染的地方

    ![img](《Real_time Rendering 3rd》读书笔记.assets/E348BA45D98415A6459DA28EEF1A246A.png)

    - 顶点着色
      - 目的：根据光源材质等对物体顶点上色
      - 方式：对每个点计算着色方程
    - 投影
      - 目的：降维：将模型从三维空间投射到二维空间
      - 两种投影变换：
        - 正交投影
        - 透视投影

    ![img](《Real_time Rendering 3rd》读书笔记.assets/18C9021400FD4F30E427E9B4582B0442.png)

    - 裁剪
      - 目的：对部分位于视体的图元裁剪
      - 图元相对视体的三个位置
        - 完全位于视体内，直接进入下一阶段
        - 部分位于视体内，裁剪
        - 不位于视体内，丢弃

    ![img](《Real_time Rendering 3rd》读书笔记.assets/0FCD986988C17CCA00A232176DFAF12F.png)

    - 屏幕映射
      - 目的：将之前步骤得到的坐标映射到对应的屏幕坐标

  ![img](《Real_time Rendering 3rd》读书笔记.assets/F35323399B630ECA64DF9B6BF656255C.png)

- 光栅化阶段

  - 细分：

    ![img](《Real_time Rendering 3rd》读书笔记.assets/E7AE4D9738B9665E27C74AA43201F64B.png)

    - 三角形设定阶段
      - 计算三角形表面的数据
    - 三角形遍历阶段
      - 找到哪些采样点或者像素在三角形中
    - 像素着色阶段
      - 逐像素的着色计算，纹理贴图在这阶段进行。

    ![img](《Real_time Rendering 3rd》读书笔记.assets/37B3884997ACBEDC44EB6E28F28C4A34.png)

    - 融合阶段
      - 从z缓冲器，颜色缓冲器，alpha通道，模板缓冲器，帧缓冲器，，累计缓冲器得到数据融合出图像到屏幕上

   

## 思维导图

![img](《Real_time Rendering 3rd》读书笔记.assets/1.png)