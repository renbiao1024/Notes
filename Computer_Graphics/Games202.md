# 高质量实时渲染

- 高质量：真实感

- 实时：30+FPS（每秒生成至少30张图）

- 渲染：模拟光源进入眼睛

## Programmable graphics hardware (shaders)

![image-20220226161744366](Games202.assets/image-20220226161744366.png)

## shadow mapping（阴影生成）

- 2通道算法

  - 从光的角度来看，得到深度缓冲器

  - 从相机角度来看，将深度贴图投影到相机的视图上

![image-20220228161614327](Games202.assets/image-20220228161614327.png)

- 自遮挡问题

  - SM记录的是每个像素的深度，不是连续的，像素间的记录互相遮盖

  - ![image-20220228163512162](Games202.assets/image-20220228163512162.png)

  - ![image-20220228163635055](Games202.assets/image-20220228163635055.png)

  - 解决方法：障碍物在一定小的范围内不做记录，可根据夹角改变范围大小

    - ![image-20220228163605810](Games202.assets/image-20220228163605810.png)

    - 新的问题：阴影偏移

![image-20220228163653609](Games202.assets/image-20220228163653609.png)

- 锯齿问题

![image-20220228164536456](Games202.assets/image-20220228164536456.png)

- Shadow map ≈ Visibility×shading

  - ![image-20220228170220515](Games202.assets/image-20220228170220515.png)
  - 准确的条件
    - 点光源/平行光
    - 漫射brdf/恒定辐射区域照明

  - 生成的是硬阴影

- PCF（Percentage Closer Filtering）用于处理阴影的锯齿，生成软阴影
  - 每个像素都取周围像素做一个平均
  - ![image-20220228171851984](Games202.assets/image-20220228171851984.png)

![image-20220228171926678](Games202.assets/image-20220228171926678.png)

- 生活中观察，物体和阴影近的地方阴影较硬，远的地方阴影变软

  - ![image-20220228172746332](Games202.assets/image-20220228172746332.png)

  - ![image-20220228172835780](Games202.assets/image-20220228172835780.png)

  - $$
    相似三角形计算 \\
    W_{penumbra}=(d_{Receiver}-d_{Blocker})\sdot W_{Light}/d_{Blocker}
    $$

- PCSS(Percentage Closer Soft Shadows) ：适应性的PCF
  - 步骤
    - 遮挡区搜索：获得某一区域的平均阻挡深度
    - 使用遮挡区的深度计确定滤波范围（和周围像素取平均的范围）
    - PCF