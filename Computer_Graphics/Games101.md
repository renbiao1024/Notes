前言：最近看了好多大佬的视频和文章，赞叹之余，做些记录。
UE工程：https://github.com/RBUEProject/ComputerGraphics
（参照：https://www.cnblogs.com/shiroe/tag/%E5%9B%BE%E5%BD%A2%E5%AD%A6/制作）这是真大佬

# 参考资料

https://www.bilibili.com/video/BV1X7411F744
https://www.bilibili.com/video/BV1Qi4y1g7xG
https://www.notion.so/GAMES101-b0e27c856cde429b8672671a54c34817

# 数学知识

简单记录

## 向量

### 点积

公式
![在这里插入图片描述](UE4ComputerGraphics.assets/a6ca76b5efbb49d49a53be5d8ec367e6.png)
性质
![在这里插入图片描述](UE4ComputerGraphics.assets/e1cbb7a7b1624959b81e258dcce67a79.png)
矩阵表示
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_17,color_FFFFFF,t_70,g_se,x_16.png)
在图形学中作用：
判断两个方向之间的大小：cos大小
分解向量
确定前后：cos正负

### 叉积

![在这里插入图片描述](UE4ComputerGraphics.assets/e55f0712df394dc0bf8001a2b4f7bef5.png)
性质：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_16,color_FFFFFF,t_70,g_se,x_16.png)
矩阵表示：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16.png)

作用：
建立直角坐标系：X轴 x Y轴 = Z轴（右手系）
判定左右
判定图形内外
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993752124.png)
分解向量
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_19,color_FFFFFF,t_70,g_se,x_16.png)
Eigen线性代数运算库

```
Eigen::Vector3f u(1.0f,2.0f,3.0f);//向量定义
Eigen::Vector3f v(1.0f,0.0f,0.0f);
v + w ; //向量求和
 2.0f * v ;//向量数乘

 Eigen::Matrix3f i,j;//矩阵定义
 i << 1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0, 8.0, 9.0;
 j << 2.0, 3.0, 1.0, 4.0, 6.0, 5.0, 9.0, 7.0, 8.0;
i + j//矩阵加减
i * 2.0//矩阵数乘
i * j//矩阵乘法
i * v//矩阵乘向量
```

## 概率论

离散随机变量 X:

- 分布函数（PDF）:X~p(x)
- 期望：E= Σxi*pi

连续随机变量X：
![在这里插入图片描述](UE4ComputerGraphics.assets/cf96b1d9a9fd4c0ea6625e165450cc2c.png)

## 矩阵

矩阵的转置：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_15,color_FFFFFF,t_70,g_se,x_16.png)
基础变换：
线性变换 = 旋转 + 缩放 + 切变
仿射变换 = 线性变换 + 平移变换

三元数表示二维矩阵
![在这里插入图片描述](UE4ComputerGraphics.assets/9e34a70c4c4f416180c993d2d4d75bec.png)

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_15,color_FFFFFF,t_70,g_se,x_16-1640877993753125.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/d009dc2b7c0e4df9b267a4f316e75894.png)

四元数表示三维变换：
![在这里插入图片描述](UE4ComputerGraphics.assets/d10eb736888b491d9388ebbdc6c7fbaa.png)

![在这里插入图片描述](UE4ComputerGraphics.assets/939b494aea584759828388a8b103649d.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993753126.png)

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993753127.png)
欧拉角表示旋转
row pitch yaw ：绕x,y,z轴旋转

绕n轴旋转α角
![在这里插入图片描述](UE4ComputerGraphics.assets/0e696bc5aa504971a338de49337c5981.png)
若绕静坐标系（世界坐标系）旋转，则左乘，也是变换矩阵坐标矩阵；若是绕动坐标系旋转（自身建立一个坐标系），则右乘，也就是坐标矩阵变换矩阵。 即，左乘是相对于坐标值所在的坐标系（世界坐标系）下的三个坐标轴进行旋转变换。而右乘则是以当前点为旋转中心，进行旋转变换。

### 例题

给定一个点 P=(2,1), 将该点绕原点先逆时针旋转 45◦，再平移 (1,2), 计算出变换后点的坐标（要求用齐次坐标进行计算）。
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993753128.jpeg)

```
#include<cmath>
#include <Eigen/Core>
#include <Eigen/Dense>
#include <iostream>

//分步版本
void Transformation_1() {
    Eigen::Vector3d v(2.0f, 1.0f, 1.0f);
    Eigen::Matrix3d rotate;
    Eigen::Matrix3d tranform;
    double r = -45.0 / 180.0 * acos(-1);//把角度转换成弧度
    rotate << cos(r), sin(r), 0,
            -1.0f * sin(r), cos(r), 0,
            0, 0, 1;
    tranform << 1, 0, 1,
                0, 1, 2,
                0, 0, 1;

    //两次左乘
    v = rotate * v;
    v = tranform * v;
}

//总变换矩阵
void Transformation_2() {
    Eigen::Vector3d v;
    Eigen::Vector3d trans;
    double r = -45.0 / 180.0 * acos(-1);
    trans << cos(r),sin(r),1,
            -1.0f * sin(r), cos(r), 2,
            0, 0, 1;
    v = trans * v;
}
```

# MVP变换



![img](UE4ComputerGraphics.assets/v2-abeba6616f1aab5637b33ba035715848_720w.jpg)

- Model transformation（模型变换）：用一个变化矩阵把actor的顶点坐标从Local坐标系（相对）转换到世界Global坐标系（绝对）。
  
- View transformation（视口变换）：将世界坐标系转换到摄像机坐标系
  - 设摄像机朝向的方向向量为 g，up direction 为 t，分解一下这个变换：
    1. 将摄像机移到原点；
    2. 将 g 旋转到 -z 方向；
    3. 将 t 旋转到 y 方向；
    4. 将 (g x t) 旋转到 x 方向；
  - ![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_17,color_FFFFFF,t_70,g_se,x_16-1640877993753130.png)

- Projection transformation（投影变换）：找到屏幕，把Actor平移缩放到屏幕上![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_15,color_FFFFFF,t_70,g_se,x_16-1640877993753131.png)
  - 透视投影变换就可以分解成如下两个步骤：
    1. 将四棱台变换成长方体。就是顶面不动，把内部、侧面以及底面向内压缩成一个长方体。
    2. 把长方体做正交投影变换。
  - ![preview](UE4ComputerGraphics.assets/v2-9f157469b7f9b7ab350765fef033a1b2_r.jpg)
  - ![在这里插入图片描述](UE4ComputerGraphics.assets/5c005622fad34ba2a3773e044ba7fb5c.png)
    ![在这里插入图片描述](UE4ComputerGraphics.assets/a644e831a0b44a81a621b37ffcf722f7.png)
  - 正交投影:对空间中![[公式]](https://www.zhihu.com/equation?tex=%5Bl%2C+r%5D+%5Ctimes+%5Bb%2C+t%5D+%5Ctimes+%5Bf%2C+n%5D)的体积成像，将其变换到 ![[公式]](https://www.zhihu.com/equation?tex=%5B-1%2C1%5D%5E3) ，那么它的变换可以分解为：先平移这个空间的中心到原点，再放缩。
    - ![在这里插入图片描述](UE4ComputerGraphics.assets/2411a90522ac4d3794f736e6d68c9d2e.png)
- 最终得到透视投影矩阵：![preview](UE4ComputerGraphics.assets/v2-eb5992872c8f8ab9317d43334a892684_r.jpg)

### 例题

填写一个旋转矩阵和一个透视投影矩阵。给定三维下三个点 v0(2.0, 0.0, −2.0), v1(0.0, 2.0, −2.0), v2(−2.0, 0.0, −2.0), 你需要将这三个点的坐标变换为屏幕坐标并在屏幕上绘制出对应的线框三角形 (在代码框架中，我们已经提供了 draw_triangle 函数，所以你只需要去构建变换矩阵即可)。简而言之，我们需要进行模型、视图、投影、视口等变换来将三角形显示在屏幕上。在提供的代码框架中，我们留下了模型变换和投影变换的部分给你去完成。
在本次作业中，因为你并不需要去使用三角形类，所以你需要理解修改的文件为:rasterizer.hpp 和 main.cpp。其中 rasterizer.hpp 文件作用是生成渲染器界面与绘制。

```
#include "Triangle.hpp"
#include "rasterizer.hpp"
#include <eigen3/Eigen/Eigen>
#include <iostream>
#include <opencv2/opencv.hpp>

constexpr double MY_PI = 3.1415926;

//模型变换
Eigen::Matrix4f get_model_matrix(float rotation_angle)
{
    Eigen::Matrix4f model = Eigen::Matrix4f::Identity();

	//绕z轴旋转矩阵
	double r = rotation_angle / 180.0*acos(-1);
	Eigen::Matrix4f translate;
	translate<<cos(r),(-1)*sin(r),0,0,
				sin(r),cos(r),0,0,
				0,0,1,0,
				0,0,0,1;
	model = translate * model;
    return model;
}

//视图变换
Eigen::Matrix4f get_view_matrix(Eigen::Vector3f eye_pos)
{
	Eigen::Matrix4f view = Eigen::Matrix4f::Identity();
	Eigen::Matrix4f translate;
	//，本项目中设定相机的初始方向已经是正确的方向，所以并不需要进行旋转摆正相机仅作了平移，把相机放到坐标原点
	translate << 1, 0, 0, -eye_pos[0],
				0, 1, 0, -eye_pos[1],
				0, 0, 1,-eye_pos[2],
				0, 0, 0, 1;

	view = translate * view;
	return view;
}

//投影变换
Eigen::Matrix4f get_projection_matrix(float eye_fov, float aspect_ratio,float zNear, float zFar)
//四个参数含义，eye_fov代表垂直的可视角度，aspect_ratio是宽高比，zNear和zFar分别为近远平面的z轴坐标。
{
    Eigen::Matrix4f projection = Eigen::Matrix4f::Identity();//定义 4*4 单位矩阵

	Eigen::Matrix4f persp_to_ortho = Eigen::Matrix4f::Identity();//透视->正交的“挤压”矩阵”
	persp_to_ortho << zNear, 0, 0, 0,
		0, zNear, 0, 0,
		0, 0, zNear + zFar, -(zNear * zFar),
		0, 0, 1, 0;

	float half_eye_fovY = eye_fov / 2 / 180.0 * MY_PI;

//屏幕范围
	float top = -zNear * tan(half_eye_fovY);
	float bottom = -top;
	float right = aspect_ratio * top;
	float left = -right;

	Eigen::Matrix4f ortho_translate = Eigen::Matrix4f::Identity();//将方块中心移到原点
	ortho_translate << 1, 0, 0, -(right + left) / 2,
		0, 1, 0, -(top + bottom) / 2,
		0, 0, 1, -(zNear + zFar) / 2,
		0, 0, 0, 1;

	Eigen::Matrix4f ortho_scale = Eigen::Matrix4f::Identity();//将方块长宽高全变成2
	ortho_scale << 2 / (right - left), 0, 0, 0,
		0, 2 / (top - bottom), 0, 0,
		0, 0, 2 / (zNear - zFar), 0,
		0, 0, 0, 1;

	projection = ortho_scale * ortho_translate * persp_to_ortho * projection;

	return projection;
}
//实现绕任意轴旋转
Eigen::Matrix4f get_rotation(Vector3f axis, float angle)
{

    Eigen::Matrix4f model = Eigen::Matrix4f::Identity();
    Eigen::Matrix3f temp = Eigen::Matrix3f::Identity();
    float ag = angle/180*MY_PI;
    Eigen::Matrix3f tr;
    Eigen::Matrix3f mul;
    mul << 0, -axis[2], axis[1],
        axis[2], 0, -axis[0],
        -axis[1], axis[0], 0;
    tr = cos(ag)*temp + (1-cos(ag))*axis*axis.adjoint() + mul*sin(ag);
    model << tr(0,0), tr(0,1), tr(0,2), 0,
            tr(1,0), tr(1,1), tr(1,2), 0,
            tr(2,0), tr(2,1), tr(2,2), 0,
            0, 0, 0, 1;
    return model;
}



int main(int argc, const char** argv)
{
    float angle = 0;
    bool command_line = false;
    std::string filename = "output.png";

    if (argc >= 3) {
        command_line = true;
        angle = std::stof(argv[2]); // -r by default
        if (argc == 4) {
            filename = std::string(argv[3]);//将旋转某角度的图像存储到本地
        }
        else
            return 0;
    }

    rst::rasterizer r(700, 700);//确定宽高参数的屏幕

    Eigen::Vector3f eye_pos = {0, 0, 5};//相机坐标

    std::vector<Eigen::Vector3f> pos{{2, 0, -2}, {0, 2, -2}, {-2, 0, -2}};//三角形三个定点的坐标

    std::vector<Eigen::Vector3i> ind{{0, 1, 2}};//三个点的索引值

	//通过loadposition与load_indices将坐标点和索引值赋给光栅化器（rasterizer)
    auto pos_id = r.load_positions(pos);
    auto ind_id = r.load_indices(ind);

    int key = 0;
    int frame_count = 0;

    if (command_line) {
        r.clear(rst::Buffers::Color | rst::Buffers::Depth);//清屏

		r.set_model(get_model_matrix(angle));//将内部的模型矩阵作为参数传递给光栅化器。
		r.set_view(get_view_matrix(eye_pos));//将视图变换矩阵设为内部视图矩阵
		r.set_projection(get_projection_matrix(45, 1, 0.1, 50));//将内部的投影矩阵设为给定矩阵 p，并传递给光栅化器

        r.draw(pos_id, ind_id, rst::Primitive::Triangle);
        cv::Mat image(700, 700, CV_32FC3, r.frame_buffer().data());
        image.convertTo(image, CV_8UC3, 1.0f);

        cv::imwrite(filename, image);

        return 0;
    }

    while (key != 27) {
        r.clear(rst::Buffers::Color | rst::Buffers::Depth);
		//MVP变换
        r.set_model(get_model_matrix(angle));
        r.set_view(get_view_matrix(eye_pos));
        r.set_projection(get_projection_matrix(45, 1, 0.1, 50));

        r.draw(pos_id, ind_id, rst::Primitive::Triangle);//绘制三角形

        cv::Mat image(700, 700, CV_32FC3, r.frame_buffer().data());
        image.convertTo(image, CV_8UC3, 1.0f);
        cv::imshow("image", image);
        key = cv::waitKey(10);

        std::cout << "frame count: " << frame_count++ << '\n';
//控制旋转
        if (key == 'a') {
            angle += 10;
        }
        else if (key == 'd') {
            angle -= 10;
        }
    }

    return 0;
}
```

# 光栅化

屏幕由像素点构成，是离散的，二维的
最基本的二维元素是三角形，内部一定是平面

## Sampling采样

### 寻找像素点

一个像素点对应一个坐标点，对这个坐标点采样，判断它在不在三角形里面，边界不做处理or特殊处理。
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993754132.png)

### 加速

方法1：直接遍历所有点浪费时间，用包围盒加速。
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993754133.png)
方法2：![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_14,color_FFFFFF,t_70,g_se,x_16.png)

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_10,color_FFFFFF,t_70,g_se,x_16.png)

采样的缺点：以点代面，有失偏颇 → Aliasing 走样，表现为锯齿
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_10,color_FFFFFF,t_70,g_se,x_16-1640877993754134.png)

### 抗锯齿

先模糊再采样
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993754135.png)
最理想的状态：像素的颜色值为负责的区域内的平均值。
但是连续域的平均值是很难求的，计算量巨大

解决方法：Supersampling（MSAA）
每个像素多次采样，求平均。太浪费性能
优化：不使用均匀分布，采样复用

![在这里插入图片描述](UE4ComputerGraphics.assets/5dc1db0dd5794dd084b06e56d334d6cf.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_11,color_FFFFFF,t_70,g_se,x_16.png)

> 目前得到广泛应用的其他方法：
> FXAA (Fast Approximate AA)：先获得有锯齿的图，再后处理去除锯齿（很快）
>
> - 找到边界，换成没有锯齿的边界，（图像匹配）非常快
> - 方法和采样无关，采样虽然有误，但是这种方法可以弥补
>
> TAA (Tem‘poral AA) ：时序信息，借助前面帧的信息
>
> - 静态场景，相邻两帧同一像素用不同的位置来sample
> - 把MSAA的Sampling分布在时间上
>
> Super resolution / super sampling 超分辨率

### 深度处理

绘制是有远近的，如何确定图形的绘制顺序？

1. 画家算法：由远及近画画，覆盖。无法处理互相遮挡的情况
   ![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_12,color_FFFFFF,t_70,g_se,x_16.png)
2. Z-buffer
   对每个像素多存一个深度缓冲区
   实际中，相机在原点往-z方向看，z越小说明越远
   ![数字代表颜色](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993754136.png)
   ![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993754137.png)
   ![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993754138.png)
   复杂度：O(n) 并不是排序，而是只要最值
   需要保证三角形进入顺序和结果无关
   无法处理透明物体

### 例题

在屏幕上画出一个实心三角形，换言之，栅格化一个三角形。你需要自己填写并调用函数 rasterize_triangle(const Triangle& t)和static bool insideTriangle()

> rasterize_triangle(const Triangle& t)内部工作流程如下：
>
> 1. 创建三角形的 2 维 bounding box。
> 2. 遍历此 bounding box 内的所有像素（使用其整数索引）。然后，使用像素中心的屏幕空间坐标来检查中心点是否在三角形内。
> 3. 如果在内部，则将其位置处的插值深度值 (interpolated depth value) 与深度缓冲区 (depth buffer) 中的相应值进行比较。
> 4. 如果当前点更靠近相机，请设置像素颜色并更新深度缓冲区 (depth buffer)。

> static bool insideTriangle()内部工作流程如下：
>
> 1. 三角形三边的向量
> 2. 测试点和三角形顶点的向量
> 3. 叉乘判断点在三角形内外

```
static bool insideTriangle(int x, int y, const Vector3f* _v)
{   
    //测试点(x,y) ， 三角形三点坐标 _v[0],_v[1],_v[2]

	//三角形各边的向量
	Eigen::Vector2f side1;
	side1<< _v[1].x() - _v[0].x(), _v[1].y() - _v[0].y();
	Eigen::Vector2f side2;
	side2 << _v[2].x() - _v[1].x(), _v[2].y() - _v[1].y();
	Eigen::Vector2f side3;
	side3 << _v[0].x() - _v[2].x(), _v[0].y() - _v[2].y();

	//测试点和三角形边界点的向量
	Eigen::Vector2f v1;
	v1 << x - _v[0].x(), y - _v[0].y();
	Eigen::Vector2f v2;
	v2 << x - _v[1].x(), y - _v[1].y();
	Eigen::Vector2f v3;
	v3 << x - _v[2].x(), y - _v[2].y();

	//三角形各边的的向量 X 测量点和三角形各点连线的向量
	//叉乘公式为(x1, y1)X(x2, y2) = x1*y2 - y1*x2
	float z1 = side1.x() * v1.y() - side1.y() * v1.x();
	float z2 = side2.x() * v2.y() - side2.y() * v2.x();
	float z3 = side3.x() * v3.y() - side3.y() * v3.x();

	//判断点在三角形 内（同号）or 外（异号）
	if ((z1 > 0 && z2 > 0 && z3 > 0) || (z1 < 0 && z2 < 0 && z3 < 0))
	{
		return true;
	}
	else
	{
		return false;
	}
}
//三角形栅格化
void rst::rasterizer::rasterize_triangle(const Triangle& t) {
    auto v = t.toVector4();
    //v是一个包含三个顶点信息的数组，每个顶点坐标用Vector4f（齐次坐标）表示

	//构建AABB包围盒
		//找到AABB边界
	float min_x = std::min(v[0].x(), std::min(v[1].x(), v[2].x()));
	float max_x = std::max(v[0].x(), std::max(v[1].x(), v[2].x()));
	float min_y = std::min(v[0].y(), std::min(v[1].y(), v[2].y()));
	float max_y = std::max(v[0].y(), std::max(v[1].y(), v[2].y()));
    
	//遍历AABB包围盒的点
	for (int x = min_x; x <= max_x; x++) {
		for (int y = min_y; y <= max_y; y++) {
			if (insideTriangle(x, y, t.v)) {
			//判断是否在三角形内, 如果在内部，则将其位置处的插值深度值(interpolated depth value) 
			//与深度缓冲区(depth buffer) 中的相应值进行比较。
				float min_depth = FLT_MAX;//初始化无限远
				//对于三角形内部的像素，我们需要用插值的方法得到其深度值。
				auto tup = computeBarycentric2D(x,y,t.v);
				float alpha,beta,gamma;
				std::tie(alpha,beta,gamma) = tup;
				float w_reciprocal = 1.0 / (alpha / v[0].w() + beta / v[1].w() + gamma / v[2].w());
				float z_interpolated = alpha * v[0].z() / v[0].w() + beta * v[1].z() / v[1].w() + gamma * v[2].z() / v[2].w();
				z_interpolated *= w_reciprocal;
				min_depth = std::min(min_depth, z_interpolated);

				if (depth_buf[get_index(x, y)] > min_depth) {
					//获得最上层应该渲染的颜色
					Vector3f color = t.getColor();
					Vector3f point;
					point <<x,y,min_depth;
					//更新深度
					depth_buf[get_index(x, y)] = min_depth;
					//更新所在点的颜色
					set_pixel(point, color);
				}
			}
		}
	}
}
```

# 着色

## Bling-Phong Reflectance Model光照模型 着色模型

漫反射：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993754139.png)
环境光：是假的，因为全局光照（GI）十分复杂，所以用环境贴图模拟
![在这里插入图片描述](UE4ComputerGraphics.assets/defc3480fa684eee85fb43cc530d683f.png)
高光：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993755140.png)

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993755141.png)
高光+漫反射+环境光 = 光照模型
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993755142.png)

## 着色模型

flat shading：对每个三角形面着色
![在这里插入图片描述](UE4ComputerGraphics.assets/3446c72ff7b642bb8198615110729a3f.png)
Gouraud shading：先对每个三角形的顶点着色，后对颜色插值
![在这里插入图片描述](UE4ComputerGraphics.assets/47b1e20fa585472ebf19e77690d71280.png)
Phong shading：先对法线值做插值，后对每个点着色（开销大）
顶点法线方向的计算：相邻面的法线的平均
![在这里插入图片描述](UE4ComputerGraphics.assets/88735344bb964dc8ab77a844a2416ed1.png)
对比：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993755143.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993755144.png)

## 渲染管线

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993755145.png)

## 纹理映射

三维表面都是二维的
纹理图可映射到三维
三角形每个顶点对应一个uv坐标
一张纹理可以使用多次：纹理本身设计可以无缝衔接

## 三角形重心坐标

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_15,color_FFFFFF,t_70,g_se,x_16-1640877993755146.png)
投影前后的重心坐标可能会变化，计算时需要结合深度。
所以需要在对应时间计算对应的重心坐标来做插值，不能随意复用！
计算出三角形内三个点对中心点的权重和他们对中心点的加权

应用材质：
像素在三角形内→计算对应uv→取纹理对应颜色值→设置

问题1：纹理过小，在大物体上被拉伸（多个像素点用一个纹素）
解决方法：双线性插值， 双向三次插值

双线性插值：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993755147.png)

双向三次插值：周围16个点做三次插值

问题2：纹理太大，一个pixel对应了多个texel → 采样频率不足导致 摩尔纹+锯齿（走样）
解决方法：Mipmap
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993755148.png)
层数D约为相邻pixel的映射uv之间的距离取2的对数
若D不是整数->三线性插值
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_19,color_FFFFFF,t_70,g_se,x_16-1640877993755149.png)

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993755150.png)
Mipmap局限性：只能正方形近似，对于某些角度、大小特别的点，处理效果不佳，产生模糊
处理方法：各向异性过滤，EWA过滤
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993755151.png)
（不太懂）

## 贴图

### Environment Map

![环境光贴图，天空球（盒）](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993756152.png)
Cube Map：立方体表面，从球心到球面的投影向外

### 法线贴图，凹凸贴图

记录了高度移动，不改变几何信息，逐像素扰动法线方向，高度 offset 相对变化，从而改变法线方向
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_14,color_FFFFFF,t_70,g_se,x_16-1640877993756153.png)
法贴扰动模型表面原理（2维）：
原来的法线：n§ = (0, 1)
扰动： dp = c * [h(p+1) - h§]
扰动后的法线： n§ = (-dp, 1)，归一化
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_19,color_FFFFFF,t_70,g_se,x_16-1640877993756154.png)

### 位移贴图

改变几何信息，对顶点做位移
相比上更逼真，要求模型足够细致，运算量更高

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_15,color_FFFFFF,t_70,g_se,x_16-1640877993756155.png)

### 环境光遮蔽贴图

- 计算好的环境光遮蔽贴图
- 空间换时间

### 过程纹理

- 定义空间中任意点的颜色
- 噪声+映射→

### 阴影映射

使用光栅化绘制阴影：
思想：不在阴影里的点必须同时被光线和相机看到

步骤：

1.从光源出发得到深度图→阴影贴图
2.从眼睛发出光得到标准图像(有深度)
3.将肉眼可见的点投射回光源
if 光源可见→颜色
if 阻塞→阴影
（下图白点可见颜色，黑点为阴影）
![在这里插入图片描述](UE4ComputerGraphics.assets/cb1589fb35bb41d4a5176272dfd53d6c.png)

产生的问题：
走样，分辨率
只能点光源
产生的是硬阴影
![在这里插入图片描述](UE4ComputerGraphics.assets/2d21bcc625e2446fba21c80978a10eb8.png)

### 例题

在代码中添加了Object Loader(用于加载三维模型), Vertex Shader 与 Fragment Shader，并且支持了纹理映射。
需要完成的任务是:

1. 修改函数 rasterize_triangle(const Triangle& t) in rasterizer.cpp: 在此处实现与作业 2 类似的插值算法，实现法向量、颜色、纹理颜色的插值。

```
void rst::rasterizer::rasterize_triangle(const Triangle& t, const std::array<Eigen::Vector3f, 3>& view_pos) 
{
	//构建三角形包围盒
	auto v = t.toVector4();
	int min_x = std::min({ v[0].x(), v[1].x(), v[2].x() });
	int max_x = std::max({ v[0].x(), v[1].x(), v[2].x() });
	int min_y = std::min({ v[0].y(), v[1].y(), v[2].y() });
	int max_y = std::max({ v[0].y(), v[1].y(), v[2].y() });

	for (int x = min_x; x <= max_x; x++)
	{
		for (int y = min_y; y <= max_y; y++)
		{
			if (insideTriangle((float)x + 0.5, (float)y + 0.5, t.v))
			{
				//插值
				auto[alpha, beta, gamma] = computeBarycentric2D(x, y, t.v);
				
				//深度插值
					//如果直接进行深度插值
				//float zp = alpha * v[0].z() + beta*v[1].z()+gamma*v[2].z();
					//直接投影时三角形重心会变，所以要使用
					//透视校正插值
				float Z = 1.0 / (alpha / v[0].w() + beta / v[1].w() + gamma / v[2].w());
				float zp = alpha * v[0].z() / v[0].w() + beta * v[1].z() / v[1].w() + gamma * v[2].z() / v[2].w();
				zp *= Z;
				
				if (zp < depth_buf[get_index(x, y)])
				{
					//实现法向量、颜色、纹理颜色的插值
					auto interpolated_color = interpolate(alpha, beta, gamma, t.color[0], t.color[1], t.color[2], 1);
					auto interpolated_normal = interpolate(alpha, beta, gamma, t.normal[0], t.normal[1], t.normal[2], 1).normalized();
					auto interpolated_texcoords = interpolate(alpha, beta, gamma, t.tex_coords[0], t.tex_coords[1], t.tex_coords[2], 1);
					//view_pos[]是三角形顶点在view space中的坐标,插值是为了还原在camera space中的坐标
					auto interpolated_shadingcoords = interpolate(alpha, beta, gamma, view_pos[0], view_pos[1], view_pos[2], 1);
//payload 存储了颜色，法线，点，相机位置等等的信息
					fragment_shader_payload payload(interpolated_color, interpolated_normal.normalized(), interpolated_texcoords, texture ? &*texture : nullptr);
					payload.view_pos = interpolated_shadingcoords;
					auto pixel_color = fragment_shader(payload);
					//更新深度缓存，并修改像素
					depth_buf[get_index(x, y)] = zp;
					set_pixel(Eigen::Vector2i(x, y), pixel_color);

				}
			}
		}
	}
}
```

1. 修改函数 phong_fragment_shader() in main.cpp: 实现 Blinn-Phong 模型计算 Fragment Color.

```
Eigen::Vector3f phong_fragment_shader(const fragment_shader_payload& payload)
{
    Eigen::Vector3f ka = Eigen::Vector3f(0.005, 0.005, 0.005);//环境光系数
    Eigen::Vector3f kd = payload.color;//漫反射系数
    Eigen::Vector3f ks = Eigen::Vector3f(0.7937, 0.7937, 0.7937);//高光系数
	//2个光源
    auto l1 = light{{20, 20, 20}, {500, 500, 500}};
    auto l2 = light{{-20, 20, 0}, {500, 500, 500}};

    std::vector<light> lights = {l1, l2};
    Eigen::Vector3f amb_light_intensity{10, 10, 10};//光强
    Eigen::Vector3f eye_pos{0, 0, 10};//相机位置

    float p = 150;

    Eigen::Vector3f color = payload.color;
    Eigen::Vector3f point = payload.view_pos;
    Eigen::Vector3f normal = payload.normal;

    Eigen::Vector3f result_color = {0, 0, 0};
    for (auto& light : lights)
    {
	//转换成相机的相对坐标系
		Eigen::Vector3f light_dir = (light.position - point).normalized();
		Eigen::Vector3f view_dir = (eye_pos - point).normalized();
		Eigen::Vector3f half_vector = (light_dir + view_dir).normalized();

		// 距离衰减
		float r2 = (light.position - point).dot(light.position - point);

		//环境光
			//cwiseProduct()：矩阵点对点相乘
		Eigen::Vector3f La = ka.cwiseProduct(amb_light_intensity);

		//漫反射
		Eigen::Vector3f Ld = kd.cwiseProduct(light.intensity / r2);
		Ld *= std::max(0.0f, normal.normalized().dot(light_dir));

		//高光
		Eigen::Vector3f Ls = ks.cwiseProduct(light.intensity / r2);
		Ls *= std::pow(std::max(0.0f, normal.normalized().dot(half_vector)), p);

		result_color += (La + Ld + Ls);
        
    }

    return result_color * 255.f;
}
```

1. 修改函数 texture_fragment_shader() in main.cpp: 在实现 Blinn-Phong的基础上，将纹理颜色视为公式中的 kd，实现 Texture Shading FragmentShader.

```
//纹理映射
Eigen::Vector3f texture_fragment_shader(const fragment_shader_payload& payload)
{
    Eigen::Vector3f return_color = {0, 0, 0};
    if (payload.texture)
    {
		return_color = payload.texture->getColor(payload.tex_coords.x(),payload.tex_coords.y());
		//phong shading的kd是从RGB中取，而texture shading的kd则是根据UV坐标从纹理中取。
    }
    Eigen::Vector3f texture_color;
    texture_color << return_color.x(), return_color.y(), return_color.z();

    Eigen::Vector3f ka = Eigen::Vector3f(0.005, 0.005, 0.005);
    Eigen::Vector3f kd = texture_color / 255.f;
    Eigen::Vector3f ks = Eigen::Vector3f(0.7937, 0.7937, 0.7937);

    auto l1 = light{{20, 20, 20}, {500, 500, 500}};
    auto l2 = light{{-20, 20, 0}, {500, 500, 500}};

    std::vector<light> lights = {l1, l2};
    Eigen::Vector3f amb_light_intensity{10, 10, 10};
    Eigen::Vector3f eye_pos{0, 0, 10};

    float p = 150;

    Eigen::Vector3f color = texture_color;
    Eigen::Vector3f point = payload.view_pos;
    Eigen::Vector3f normal = payload.normal;

    Eigen::Vector3f result_color = {0, 0, 0};

    for (auto& light : lights)
    {
		Eigen::Vector3f light_dir = (light.position - point).normalized();
		Eigen::Vector3f view_dir = (eye_pos - point).normalized();
		Eigen::Vector3f half_vector = (light_dir + view_dir).normalized();

		// 距离衰减
		float r2 = (light.position - point).dot(light.position - point);

		//环境光
		//cwiseProduct()：矩阵点对点相乘
		Eigen::Vector3f La = ka.cwiseProduct(amb_light_intensity);

		//漫反射
		Eigen::Vector3f Ld = kd.cwiseProduct(light.intensity / r2);
		Ld *= std::max(0.0f, normal.normalized().dot(light_dir));

		//高光
		Eigen::Vector3f Ls = ks.cwiseProduct(light.intensity / r2);
		Ls *= std::pow(std::max(0.0f, normal.normalized().dot(half_vector)), p);

		result_color += (La + Ld + Ls);
    }
    return result_color * 255.f;
}
```

1. 修改函数 bump_fragment_shader() in main.cpp: 在实现 Blinn-Phong 的基础上，实现 Bump mapping.

```
//法线贴图
Eigen::Vector3f bump_fragment_shader(const fragment_shader_payload& payload)
{
    
    Eigen::Vector3f ka = Eigen::Vector3f(0.005, 0.005, 0.005);
    Eigen::Vector3f kd = payload.color;
    Eigen::Vector3f ks = Eigen::Vector3f(0.7937, 0.7937, 0.7937);

    auto l1 = light{{20, 20, 20}, {500, 500, 500}};
    auto l2 = light{{-20, 20, 0}, {500, 500, 500}};

    std::vector<light> lights = {l1, l2};
    Eigen::Vector3f amb_light_intensity{10, 10, 10};
    Eigen::Vector3f eye_pos{0, 0, 10};

    float p = 150;

    Eigen::Vector3f color = payload.color; 
    Eigen::Vector3f point = payload.view_pos;
    Eigen::Vector3f normal = payload.normal;


    float kh = 0.2, kn = 0.1;

	//求法线贴图的新法线
	float x = normal.x();
	float y = normal.y();
	float z = normal.z();

	Eigen::Vector3f t = Eigen::Vector3f(x * y / std::sqrt(x * x + z * z), std::sqrt(x * x + z * z), z * y / std::sqrt(x * x + z * z));
	Eigen::Vector3f b = normal.cross(t);

	Eigen::Matrix3f TBN;
	TBN <<
		t.x(), b.x(), normal.x(),
		t.y(), b.y(), normal.y(),
		t.z(), b.z(), normal.z();

	float u = payload.tex_coords.x();
	float v = payload.tex_coords.y();
	float w = payload.texture->width;
	float h = payload.texture->height;

	float dU = kh * kn * (payload.texture->getColor(u + 1.0f / w, v).norm() - payload.texture->getColor(u, v).norm());
	float dV = kh * kn * (payload.texture->getColor(u, v + 1.0f / h).norm() - payload.texture->getColor(u, v).norm());

	Eigen::Vector3f ln = Eigen::Vector3f(-dU, -dV, 1.0f);

	point += (kn * normal * payload.texture->getColor(u, v).norm());

	normal = (TBN * ln).normalized();

    Eigen::Vector3f result_color = {0, 0, 0};
    result_color = normal;

	//Bling_Phong光照
	for (auto& light : lights)
	{
		//Bling_Phong光照模型 过程
		result_color += (La + Ld + Ls);
	}
    return result_color * 255.f;
}
```

1. 修改函数 displacement_fragment_shader() in main.cpp: 在实现 Bump mapping 的基础上，实现 displacement mapping.

```
//位移贴图
Eigen::Vector3f displacement_fragment_shader(const fragment_shader_payload& payload)
{
    
    Eigen::Vector3f ka = Eigen::Vector3f(0.005, 0.005, 0.005);
    Eigen::Vector3f kd = payload.color;
    Eigen::Vector3f ks = Eigen::Vector3f(0.7937, 0.7937, 0.7937);

    auto l1 = light{{20, 20, 20}, {500, 500, 500}};
    auto l2 = light{{-20, 20, 0}, {500, 500, 500}};

    std::vector<light> lights = {l1, l2};
    Eigen::Vector3f amb_light_intensity{10, 10, 10};
    Eigen::Vector3f eye_pos{0, 0, 10};

    float p = 150;

    Eigen::Vector3f color = payload.color; 
    Eigen::Vector3f point = payload.view_pos;
    Eigen::Vector3f normal = payload.normal;

    float kh = 0.2, kn = 0.1;
    
	float x = normal.x();
	float y = normal.y();
	float z = normal.z();

	Eigen::Vector3f t = Eigen::Vector3f(x * y / std::sqrt(x * x + z * z), std::sqrt(x * x + z * z), z * y / std::sqrt(x * x + z * z));
	Eigen::Vector3f b = normal.cross(t);

	Eigen::Matrix3f TBN;
	TBN <<
		t.x(), b.x(), normal.x(),
		t.y(), b.y(), normal.y(),
		t.z(), b.z(), normal.z();

	float u = payload.tex_coords.x();
	float v = payload.tex_coords.y();
	float w = payload.texture->width;
	float h = payload.texture->height;

	float dU = kh * kn * (payload.texture->getColor(u + 1.0f / w, v).norm() - payload.texture->getColor(u, v).norm());
	float dV = kh * kn * (payload.texture->getColor(u, v + 1.0f / h).norm() - payload.texture->getColor(u, v).norm());

	Eigen::Vector3f ln = Eigen::Vector3f(-dU, -dV, 1.0f);
	normal = TBN * ln;


    Eigen::Vector3f result_color = {0, 0, 0};

    for (auto& light : lights)
    {
        //Bling_Phong光照模型 过程
        result_color += (La + Ld + Ls);
    }
	result_color = normal.normalized();

    return result_color * 255.f;
}
```

原模型
![原模型](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_12,color_FFFFFF,t_70,g_se,x_16-1640877993756156.png)
Bling_Phong光照
![Bling_Phong模型](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_13,color_FFFFFF,t_70,g_se,x_16.png)
材质贴图
![材质贴图](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993756157.png)
左边是正常的材质贴图，右边是将小材质贴图拉伸后的表现，中间是小材质贴图二次插值后的模糊效果

法线贴图
![法线贴图](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_14,color_FFFFFF,t_70,g_se,x_16-1640877993756158.png)
位移贴图
![位移贴图](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_14,color_FFFFFF,t_70,g_se,x_16-1640877993756159.png)

# 几何&曲线/曲面

## 几何的表示

隐式：
f(x)=0 , 满足该函数的所有点的集合就是几何体的边界
若带入的点 f(xi)<0,则点在面的内部，反之点在面的外部
不直观

显式：
1.直接给点：所有点的集合就是面

参数映射 ：(u,v)->(x,y,z)
![在这里插入图片描述](UE4ComputerGraphics.assets/ad38270b616a450ea246ef16cb3299cf.png)

2.CSG（Constructive Solid Geometry）：基本几何体的运算
![在这里插入图片描述](UE4ComputerGraphics.assets/eac55c2157e84a559e7d35c7646486de.png)
3.距离函数Distance Function
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_15,color_FFFFFF,t_70,g_se,x_16-1640877993756160.png)
4.水平集Level Set：扫描寻找边界
![在这里插入图片描述](UE4ComputerGraphics.assets/d6487d272d6c4800a857cb6da1ed196c.png)
5.分形：递归方法处理自相似问题

## 描述几何的方法

### B样条延伸

基函数样条B-spline，可局部调整
![在这里插入图片描述](UE4ComputerGraphics.assets/4ad92b8cccd843a3894d4f0dc9b00b26.png)

### 三角面

![在这里插入图片描述](UE4ComputerGraphics.assets/22dae618b5074ff6bf877cdb4b3484e0.png)

### 点云

![在这里插入图片描述](UE4ComputerGraphics.assets/eee3fea159d54d64bb948988098a8f10.png)

### 贝塞尔曲面

![在这里插入图片描述](UE4ComputerGraphics.assets/128f85804ea9485eac932fa8b77c0ca0.png)
一条由≥3个点定义的曲线：

- b0和b2定义起点和终点
- b1定义起点与终点的切线方向
- 随着时间t ，b01,b11,b02分别在b0 ~ b1，b1 ~ b2，b01 ~ b11上运动，b02走过的路径就是贝塞尔曲线。
  ![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993757161.png)

输入4个点的情况：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993757162.png)
贝塞尔曲线算法：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_19,color_FFFFFF,t_70,g_se,x_16-1640877993757163.png)
性质：
先贝塞尔后仿射 == 先仿射后贝塞尔
凸包性：曲线一定在控制点的凸包内
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_12,color_FFFFFF,t_70,g_se,x_16-1640877993757164.png)
遇到的问题：高阶贝塞尔曲线（n很大），很难控制
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_11,color_FFFFFF,t_70,g_se,x_16-1640877993757165.png)
解决方法：逐段贝塞尔曲线，每四个点控制一段曲线
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_10,color_FFFFFF,t_70,g_se,x_16-1640877993757166.png)
连续性保证：
前一个贝塞尔曲线的末尾控制点和后一个贝塞尔曲线的首个控制点重合
![在这里插入图片描述](UE4ComputerGraphics.assets/1205741af4244e7db0c7676f4efe76b7.png)
光滑性保证：
在重合的控制点的前后切线共线等长
![在这里插入图片描述](UE4ComputerGraphics.assets/a1df41ae8ca54f2bb518b24428e01c2a.png)

## 曲面

### 贝塞尔曲面：

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_11,color_FFFFFF,t_70,g_se,x_16-1640877993757167.png)
两个不同时间t（u,v）
4x4个点，四条4个控制点的贝塞尔曲线，取同一时间（比如说u）获得四个控制点，取时间v，即获得最后的曲面上的点
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_13,color_FFFFFF,t_70,g_se,x_16-1640877993757168.png)

### Mesh

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_8,color_FFFFFF,t_70,g_se,x_16.png)

曲面细分
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_9,color_FFFFFF,t_70,g_se,x_16.png)

曲面简化
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_8,color_FFFFFF,t_70,g_se,x_16-1640877993757169.png)
曲面标准化：不会出现奇怪的三角形
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_8,color_FFFFFF,t_70,g_se,x_16-1640877993757170.png)

### Loop细分

loop是人名，和循环无关

细分步骤：
创建更多的三角形：将每个三角形分成4个
根据 度 分别改变新点和旧点的位置
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_14,color_FFFFFF,t_70,g_se,x_16-1640877993757171.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_15,color_FFFFFF,t_70,g_se,x_16-1640877993757172.png)

### Catmull-Clark 细分

处理非四边形面，或者说奇异点（连线数!=4）
把奇异点变成非奇异点
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_9,color_FFFFFF,t_70,g_se,x_16-1640877993758173.png)
从中线上的中点到面的中点连线，得到所有面都是四边形
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_19,color_FFFFFF,t_70,g_se,x_16-1640877993758174.png)

奇异点在第一次细分增加[非四边形面数量]，后续不变；所有非四边形面在第一次细分都会消失

![在这里插入图片描述](UE4ComputerGraphics.assets/16ff6b43b52a4c5eac05bcadf471ddfd.png)
Bezier 曲线的反走样：
对于一个曲线上的点，不只把它对应于一个像素，你需要根据到像素中心的距离来考虑与它相邻的像素的颜色。
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_17,color_FFFFFF,t_70,g_se,x_16-1640877993758175.png)

![在这里插入图片描述](UE4ComputerGraphics.assets/064e93f2d7d64d1db50320bbce4b2b6e.png)

### 曲面简化

移动端，远距离使用复杂几何不合适
需要简化几何
1.边塌缩，顶点合并：二次误差度量（新点到面的距离和最小）
![在这里插入图片描述](UE4ComputerGraphics.assets/9eeda10e4a774c1dab0333f7b22441f5.png)
一条边的操作会影响其它边，需要更新

### 例题

实
现 de Casteljau 算法来绘制由 4 个控制点表示的 Bézier 曲线

> • bezier：该函数实现绘制 Bézier 曲线的功能。它使用一个控制点序列和一个
> OpenCV：：Mat 对象作为输入，没有返回值。它会使 t 在 0 到 1 的范围内进
> 行迭代，并在每次迭代中使 t 增加一个微小值。对于每个需要计算的 t，将
> 调用另一个函数 recursive_bezier，然后该函数将返回在 Bézier 曲线上 t
> 处的点。最后，将返回的点绘制在 OpenCV ：：Mat 对象上。
> • recursive_bezier：该函数使用一个控制点序列和一个浮点数 t 作为输入，
> 实现 de Casteljau 算法来返回 Bézier 曲线上对应点的坐标。

```
//递归贝塞尔
cv::Point2f recursive_bezier(const std::vector<cv::Point2f> &control_points, float t)
{
	//该函数实现de CasteljaudeCasteljau 算法获取每次 t 对应的点的坐标，该算法的步骤如下：
		//考虑一个p0, p1, ..., pnp0, p1, ..., pn 为控制点序列的BézierBezier 曲线。首先，将相邻的点连接起来以形成线段。
		//用 t : (1 − t) 的比例细分每个线段，并找到该分割点。
		//得到的分割点作为新的控制点序列，新序列的长度会减少一。
		//如果序列只包含一个点，则返回该点并终止。否则，使用新的控制点序列并
		//转到步骤 1。

	std::vector<cv::Point2f> points = control_points;
	// 当序列大于三个点时会继续迭代
	while (points.size() >= 3)
	{
		std::vector<cv::Point2f> points_temp;
		// 找到每个点按  t : (1 − t)  比例划分的新的顶点
		for (int i = 0; i < points.size() - 1; i++)
		{
			cv::Point2f first = points[i];
			cv::Point2f second = points[i + 1];
			cv::Point2f mid = (1 - t) * first + t * second;
			points_temp.push_back(mid);
		}
		points = points_temp;
	}
	// 序列小于三个点时，此时序列中只有两个点，连接两点，按  t : (1 − t) 取中点，那么该中点就为结束点
	cv::Point2f start = points[0];
	cv::Point2f end = points[points.size() - 1];
	cv::Point2f mid = (1 - t) * start + t * end;
	return mid;
}

//反走样
void MSAA_4X(float x, float y, cv::Mat &window)
{
	int min_x = std::max(0, static_cast<int>(floor(x)));
	int max_x = std::min(window.cols - 1, static_cast<int>(ceil(x)));
	int min_y = std::max(0, static_cast<int>(floor(y)));
	int max_y = std::min(window.rows - 1, static_cast<int>(ceil(y)));

	window.at<cv::Vec3b>(y, x)[1] = 255;
	double res = 0;
	double sigma = 1.0; // 高斯权重因子
	double weight_total = 0;

	for (int i = min_x; i <= max_x; i++)
	{
		for (int j = min_y; j <= max_y; j++)
		{
			// 邻域像素位置到红点的距离
			double dist = sqrt(pow(x - i, 2) + pow(y - j, 2));
			// 距离权重（高斯权重）
			double weight = exp(-dist / sigma);
			int colors = window.at<cv::Vec3b>(j, i)[1];

			res += static_cast<double>(colors) * weight;
			weight_total += weight;
		}
	}
	res /= weight_total;
	window.at<cv::Vec3b>(y, x)[1] = static_cast<int>(res);
}

void bezier(const std::vector<cv::Point2f> &control_points, cv::Mat &window)
{
	for (double t = 0.0; t <= 1.0; t += 0.001)
	{
		auto point = recursive_bezier(control_points, t);

		// 调用4XMSAA反走样画点
		MSAA_4X(point.x, point.y, window);
	}
}
```

# 光线追踪

由于光栅化很难处理全局光照，只是一种快速近似方法，质量低
光线追踪准确，但是很慢

## 光线追踪的过程：

1. 假设光沿直线传播，光线之间交叉不会碰撞，光路可逆（光源到摄像机==摄像机到光源）
2. 从眼睛发出一条视光，经过屏幕像素点射向场景，碰撞到物体，shading计算颜色
3. 从碰撞点发出一条到光源的光线，如果无其他碰撞，则不产生阴影，如果碰到其他物体，则产生阴影
4. 光线折射反射多次，得到的颜色值累加就是像素点最后的颜色
   ![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_17,color_FFFFFF,t_70,g_se,x_16-1640877993758176.png)
   ![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_16,color_FFFFFF,t_70,g_se,x_16-1640877993758177.png)

## 光线方程

一条有光源和方向的随时间变化的射线

![在这里插入图片描述](UE4ComputerGraphics.assets/bd09af944a224b4e9f65886c42b20587.png)
光线和球相交
联立：
![在这里插入图片描述](UE4ComputerGraphics.assets/bbd972889771432ea8ca7ce4069fac3b.png)
得：
![在这里插入图片描述](UE4ComputerGraphics.assets/28c6ccd0e22a4bda84dd6d31a5578b98.png)
求t：t是正实数
![在这里插入图片描述](UE4ComputerGraphics.assets/cec620e8887a47a09078fc30da6c098e.png)
t有两个值，拿小的那个
![在这里插入图片描述](UE4ComputerGraphics.assets/2c78fde114d142a7abf44816e9f5078c.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/f1bde5dc438b43c69ac02b504164e35e.png)

## 光线和显式表面求交

即和物体的每三角面求交
等价于：光线和平面有交点且交点在三角形内部
![在这里插入图片描述](UE4ComputerGraphics.assets/9ed491bb0bfa4b69ae121518d5b4804a.png)
求交点：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_17,color_FFFFFF,t_70,g_se,x_16-1640877993758178.png)
Möller Trumbore Algorithm: (将上式的 p’ 换成三角形的重心坐标)
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_14,color_FFFFFF,t_70,g_se,x_16-1640877993758179.png)
判断合理：t为正
判断在三角形内：重心坐标三个系数均为正

## 加速

每根光线和所有三角形求交，显然很慢
用包围盒加速——kdtree
原理：光线没有碰到包围盒就一定不会碰到内部的几何体

### AABB包围盒（轴对齐包围盒）

轴对齐为了方便分解成三对面，简化运算
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_17,color_FFFFFF,t_70,g_se,x_16-1640877993758180.png)

三对面的交集：找到Xmin,Xmax,Ymin,Ymax,Zmin,Zmax
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_8,color_FFFFFF,t_70,g_se,x_16-1640877993758181.png)
判断求交：光线与三对面的交点：三组(tmin,tmax) 如果有交集（即有同时在三对面内的时间），则与盒子有交
t 一定要为正！！！
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993759182.png)
进入所有对面才进入盒子，离开一个对面就离开了包围盒
![在这里插入图片描述](UE4ComputerGraphics.assets/c5025786a37c47d39751910a049d11d2.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/40dcc3bc967048f0b835e5291e775f70.png)

### 加速网格

步骤：

1. 求包围盒
2. 构建网格
3. 将每个对象存储在重叠的单元格中
   ![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_9,color_FFFFFF,t_70,g_se,x_16-1640877993759183.png)
4. 以光线移动方向进入网格
5. 判断重叠网格里的午踢和光线是否相交
   ![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_10,color_FFFFFF,t_70,g_se,x_16-1640877993759184.png)
   注意：格子不能太密集/稀疏
   ![在这里插入图片描述](UE4ComputerGraphics.assets/dc38eabe83884a5f91aabd1bf43a38e1.png)

### 空间划分

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_16,color_FFFFFF,t_70,g_se,x_16-1640877993759185.png)
Oct-tree 八叉树（三维均匀切分）（与维数有关）二维均分用四叉树
KD-Tree 每次只沿某一个轴划分
BSP-Tree 每次取一个方向（非横平竖直）将空间分为两部分

KD-Tree 光线判断：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_17,color_FFFFFF,t_70,g_se,x_16-1640877993759186.png)

- 光线从根结点开始向下遍历，若有交点则深入（可能与其子节点都有交点，继续判断），若无交点则离开（不可能与其子节点有交点），碰到叶子节点则与其中所有物体求交
  遇到的问题：
  一个物体可能存在于多个叶子节点中
  三角形的的点都不在Box内，但有一条边在Box内

### 分堆（BVH）

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993759187.png)
步骤：
找到包围盒
拆成两部分
细化包围盒
足够即停（每份只有少量物体）
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993760188.png)

### 例题

需要实现两个部分：光线的生成和光线与三角的相交。
需要修改：
• Renderer.cpp 中的 Render()：这里你需要为每个像素生成一条对应的光线，然后调用函数 castRay() 来得到颜色，最后将颜色存储在帧缓冲区的相应像素中。

```
void Renderer::Render(const Scene& scene)
{
    std::vector<Vector3f> framebuffer(scene.width * scene.height);

    float scale = std::tan(deg2rad(scene.fov * 0.5f));
    float imageAspectRatio = scene.width / (float)scene.height;

    // Use this variable as the eye position to start your rays.
    Vector3f eye_pos(0);
    int m = 0;

    for (int j = 0; j < scene.height; ++j)
    {
        for (int i = 0; i < scene.width; ++i)
        {
            //划分像素点
            float x;
            float y;
			x = (((i + 0.5) / ((float)scene.width) * 2) - 1) * imageAspectRatio * scale;
			y = (1 - (j + 0.5) / (float)scene.height * 2)* scale;
			//给每个像素发射一条光线
            Vector3f dir = Vector3f(x, y, -1); 
            framebuffer[m++] = castRay(eye_pos, dir, scene, 0);
        }
        UpdateProgress(j / (float)scene.height);
    }

    // save framebuffer to file
    FILE* fp = fopen("binary.ppm", "wb");
    (void)fprintf(fp, "P6\n%d %d\n255\n", scene.width, scene.height);
    for (auto i = 0; i < scene.height * scene.width; ++i) {
        static unsigned char color[3];
        color[0] = (char)(255 * clamp(0, 1, framebuffer[i].x));
        color[1] = (char)(255 * clamp(0, 1, framebuffer[i].y));
        color[2] = (char)(255 * clamp(0, 1, framebuffer[i].z));
        fwrite(color, 1, 3, fp);
    }
    fclose(fp);    
}
```

• Triangle.hpp 中的 rayTriangleIntersect(): v0, v1, v2 是三角形的三个顶点，orig 是光线的起点，dir 是光线单位化的方向向量。tnear, u, v 是你需要使用我们课上推导的 Moller-Trumbore 算法来更新的参数。

```
//求光线和三角形交点，带入Moller-Trumbore 算法即可
bool rayTriangleIntersect(const Vector3f& v0, const Vector3f& v1, const Vector3f& v2, const Vector3f& orig,
                          const Vector3f& dir, float& tnear, float& u, float& v)
{
	Vector3f E1 = v1 - v0;
	Vector3f E2 = v2 - v0;
	Vector3f S = orig - v0;
	Vector3f S1 = crossProduct(dir, E2);
	Vector3f S2 = crossProduct(S, E1);

	float temp = dotProduct(E1, S1);
	if (temp == 0 || temp < 0)
		return false;

	u = dotProduct(S1, S) / temp;

	v = dotProduct(dir, S2) / temp;

	tnear = dotProduct(S2, E2) / temp;

	if (u >= 0 && v >= 0 && tnear >= 0 && (1 - u - v) >= -__FLT_EPSILON__)//>=0 在阴影会出现蓝点
	{
		return true;
	}
	else {
		//if(u >= 0 && v >= 0 && tnear >= 0 &&(1 - u -v) < 0)
		//std::cout << u << v << tnear << std::endl;
		return false;
	}
    return false;
}
```

•实现 Ray-BoundingVolume 求交与 BVH 加速查找。

实现以下函数：
• IntersectP(const Ray& ray, const Vector3f& invDir,const std::array<int, 3>& dirIsNeg) in the Bounds3.hpp: 这个函数的作用是判断包围盒 BoundingBox 与光线是否相交

```
inline bool Bounds3::IntersectP(const Ray& ray, const Vector3f& invDir,
                                const std::array<int, 3>& dirIsNeg) const
{
//寻找AABB边界
	float t_Min_x = (pMin.x - ray.origin.x)*invDir[0];//*invDir == /Dir
	float t_Min_y = (pMin.y - ray.origin.y)*invDir[1];
	float t_Min_z = (pMin.z - ray.origin.z)*invDir[2];
	float t_Max_x = (pMax.x - ray.origin.x)*invDir[0];
	float t_Max_y = (pMax.y - ray.origin.y)*invDir[1];
	float t_Max_z = (pMax.z - ray.origin.z)*invDir[2];
	if (!dirIsNeg[0])
	{
		float t = t_Min_x;
		t_Min_x = t_Max_x;
		t_Max_x = t;
	}
	if (!dirIsNeg[1])
	{
		float t = t_Min_y;
		t_Min_y = t_Max_y;
		t_Max_y = t;
	}
	if (!dirIsNeg[2])
	{
		float t = t_Min_z;
		t_Min_z = t_Max_z;
		t_Max_z = t;
	}
	//所有的都进去才是进去，有一个出来就是出来
	float t_enter = std::max(t_Min_x, std::max(t_Min_y, t_Min_z));
	float t_exit = std::min(t_Max_x, std::min(t_Max_y, t_Max_z));
	if (t_enter < t_exit&&t_exit >= 0)
		return true;
	else
		return false;
}
```

• getIntersection(BVHBuildNode* node, const Ray ray)in BVH.cpp: 建
立 BVH 之后，我们可以用它加速求交过程。该过程递归进行，你将在其中调
用你实现的 Bounds3::IntersectP

```
Intersection BVHAccel::getIntersection(BVHBuildNode* node, const Ray& ray) const
{//遍历子节点判断相交
	Intersection intersect;
	Vector3f invdir(1. / ray.direction.x, 1. / ray.direction.y, 1. / ray.direction.z);
	std::array<int, 3> dirIsNeg;
	dirIsNeg[0] = ray.direction.x > 0;
	dirIsNeg[1] = ray.direction.y > 0;
	dirIsNeg[2] = ray.direction.z > 0;
	if (!node->bounds.IntersectP(ray, invdir, dirIsNeg))
	{
		return intersect;//不相交直接返回
	}
	if (node->left == nullptr && node->right == nullptr)
	{//到叶子节点直接返回
		return node->object->getIntersection(ray);
	}
	//递归左右子节点
	Intersection h1 = getIntersection(node->left, ray);
	Intersection h2 = getIntersection(node->right, ray);
	return h1.distance < h2.distance ? h1 : h2;
	return intersect;
}
```

# 辐射度量学

## 基本物理量

|                      | 定义                                     | 符号   | 单位                   | 计算                      |
| -------------------- | ---------------------------------------- | ------ | ---------------------- | ------------------------- |
| Radiant energy：能量 | 电磁辐射的总能量                         | Q      | J（焦耳）              |                           |
| Radiant flux：功率   | 单位时间内发射、反射、传输或接收的能量。 | Φ      | W(瓦特)，Lm(流明)      | Φ=Q/t                     |
| 立体角               | 面积/半径^2                              | Ω      | sr                     | Ω=A/r^2                   |
| Radiant Intensity    | 点光源发出的单位立体角的功率。           | I(ω)   | W/sr，lm/sr=cd(坎德拉) | I(ω)=d(Φ)/d(ω)            |
| Irradiance           | 单位面积功率(垂直/投影)                  | E(x)   | W/m^2 ，lm/m^2 = lux   | E(x)=d(Φ(x))/d(A)         |
| Radiance             | 单位立体角功率，单位投影面积功率。       | L(p,ω) | W^2/(sr * m^2)         | L(p,ω)=d^2Φ(p,w)/dωdAcosθ |

Irradiance和Radiance之间的区别就在于是否有方向性

- Irradiance: 四面八方的光线积分起来
- Radiance: 某个小面接受来自某个方向的光线or某个小面发出向某个方向的光线
  ![在这里插入图片描述](UE4ComputerGraphics.assets/e1963157a6c14132949770e4691f832a.png)

## 双向反射分布函数（BRDF）

表示每个出射方向从入射方向反射多少光
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_15,color_FFFFFF,t_70,g_se,x_16-1640877993760189.png)

## 反射方程

- 针对一个输出源（着色点），积分所有方向输入源（光照）的BRDF，获得最后的输出
  ![在这里插入图片描述](UE4ComputerGraphics.assets/79944ae7ac2d4526a63e06b96b148953.png)

## 渲染方程

包括了物体自己会发光的情况，包括了所有光线的传播情况
![在这里插入图片描述](UE4ComputerGraphics.assets/55365f0391124155a56106874f1f2718.png)
L= 自发光+反射光
![在这里插入图片描述](UE4ComputerGraphics.assets/3d7f3e658efc4140b26f536ae2f059ac.png)
简写：
![在这里插入图片描述](UE4ComputerGraphics.assets/c3bb61b2acb54569a8506d1ef315ea70.png)
抽象：
L = E + KL

## 全局光照方程

![在这里插入图片描述](UE4ComputerGraphics.assets/adad932b5df3449aa9c4cc00af7d2e23.png)

- 光栅化的着色过程只有直接光照（间接光照需要间接计算）
- 全局光照：直接和间接光照的集合
- 会收敛到一个亮度

# 路径追踪

## 蒙特卡洛积分：随机采样，靠 “蒙" 来近似

![在这里插入图片描述](UE4ComputerGraphics.assets/1b713b8821a548b08bb1dd6b55ebc913.png)
N越大结果越精确

## 光线追踪

只考虑直接光照
![在这里插入图片描述](UE4ComputerGraphics.assets/42fcca0dbb154f0383f641997319d9e7.png)
若也要考虑间接光照，只需往下递归
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993760190.png)
这就是支持全局光照的光线追踪算法。
但是光线数量呈指数增长，解决方法：每次只打出一条光线(N=1) ，就得到：

## 路径追踪

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993760191.png)
但这样会产生噪音，解决方法：每个像素取多路径求平均
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993760192.png)
生成射线步骤：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993760193.png)
优化后的路径追踪：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993760194.png)
新的问题，光线无限反射，算法不会停止
解决方法：俄罗斯轮盘赌：设定一个概率p，对每条光线进行判断，如果为真，继续发出光线，否则，就停止。
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993760195.png)
结果已经是正确的了，但由于着色点找光源靠的是运气，细分的越多找到的概率越大。所以最终效果不好，也不高效。
解决方法：把对x点的积分转换成对光源的积分
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_17,color_FFFFFF,t_70,g_se,x_16-1640877993760196.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/3930af98e91942a08ea4213451feed0a.png)
此时x的光来自两部分
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993761197.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993761198.png)
如果物体和光源之间有物体阻挡
解决方法：
![在这里插入图片描述](UE4ComputerGraphics.assets/2d02aa9eac61458186f5f615468a1021.png)### 例题
实现路径追踪算法
castRay(const Ray ray, int depth) in Scene.cpp: 是用来实现 Path Tracing 算法的，它会用到以下几个函数/变量：

intersect(const Ray ray) in Scene.cpp: 求一条光线与场景的交点

sampleLight(Intersection pos, float pdf) in Scene.cpp: 在场景的所有光源上按面积 uniform 地 sample 一个点，并计算该 sample 的概率密度

sample(const Vector3f wi, const Vector3f N) in Material.cpp: 按照该材质的性质，给定入射方向与法向量，用某种分布采样一个出射方向

pdf(const Vector3f wi, const Vector3f wo, const Vector3f N) in Material.cpp: 给定一对入射、出射方向与法向量，计算 sample 方法得到该出射方向的概率密度

eval(const Vector3f wi, const Vector3f wo, const Vector3f N) in Material.cpp: 给定一对入射、出射方向与法向量，计算这种情况下的 f_r 值

RussianRoulette in Scene.cpp: P_RR, Russian Roulette 的概率

```
Vector3f Scene::castRay(const Ray &ray, int depth) const
{
     Vector3f L_dir = { 0,0,0 };
     Vector3f L_indir = { 0,0,0 };
 
     Intersection intersection = Scene::intersect(ray);
     if (!intersection.happened)
     {
         return {};
     }
     //打到光源
     if (intersection.m->hasEmission())
     return intersection.m->getEmission();
 
     //打到物体后对光源均匀采样
     Intersection lightpos;
     float lightpdf = 0.0f;
     sampleLight(lightpos, lightpdf);//获得对光源的采样，包括光源的位置和采样的pdf
 
     Vector3f collisionlight = lightpos.coords - intersection.coords;
     float dis = collisionlight.squrenorm();
     Vector3f collisionlightdir = collisionlight.normalized();
     Ray objray(intersection.coords, collisionlightdir);
 
     Intersection ishaveobj = Scene::intersect(objray);
     //L_dir = L_i * f_r * cos_theta * cos_theta_x / |x - p | ^ 2 / pdf_light
     if (ishaveobj.distance - collisionlight.norm() > -EPSILON)//说明之间没有遮挡
     L_dir = lightpos.emit * intersection.m->eval(ray.direction, collisionlightdir, intersection.normal) * dotProduct(collisionlightdir, intersection.normal) * dotProduct(-collisionlightdir, lightpos.normal) / dis / lightpdf;
 	 打到物体后对半圆随机采样使用RR算法
     if (get_random_float() > RussianRoulette)
     return L_dir;
 
     Vector3f w0 = intersection.m->sample(ray.direction, intersection.normal).normalized();
     Ray objrayobj(intersection.coords, w0);
     Intersection islight = Scene::intersect(objrayobj);
     // shade(q, wi) * f_r * cos_theta / pdf_hemi / P_RR
     if (islight.happened && !islight.m->hasEmission())
     {
         float pdf = intersection.m->pdf(ray.direction, w0, intersection.normal);
         L_indir = castRay(objrayobj, depth + 1) * intersection.m->eval(ray.direction, w0, intersection.normal) * dotProduct(w0, intersection.normal) / pdf / RussianRoulette;
     }
 
     return L_dir + L_indir;
}
```

# 材质

## 漫反射

粗糙表面会产生漫反射，表现为将光线平均地像四周发出

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_14,color_FFFFFF,t_70,g_se,x_16-1640877993761199.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/a526c63bfc764c2a976bb371f74710ad.png)

## 镜面反射

光滑表面会产生镜面反射
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_15,color_FFFFFF,t_70,g_se,x_16-1640877993761200.png)

## 折射

![在这里插入图片描述](UE4ComputerGraphics.assets/5849532fc8ff4cb8a9a7ba1faa2b5544.png)
当入射介质的折射率大于反射介质的折射率会发生全反射。

## 反射

![在这里插入图片描述](UE4ComputerGraphics.assets/77f1f4e9e9594d1db1520731df2e60df.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/9e91c40c2add4f0788f0718e61270a1a.png)
BSDF(散射)=BRDF(反射)+BTDF(折射)

## 菲涅尔项

绝缘体：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993761201.png)
导体：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_16,color_FFFFFF,t_70,g_se,x_16-1640877993761202.png)

## 微表面材质

微表面理论：离得足够远的时候，微小的东西往往看不见，看见的是最后汇聚起来总体的样子
微表面法线分布决定粗糙程度
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993761203.png)
微表面模型：
![在这里插入图片描述](UE4ComputerGraphics.assets/dfd143427a0f42058d7e61dceeac3152.png)
F(i,h):菲涅尔项
G(i,o,h):修正平行角
D(h):法线分布

微表面的方向性：
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_16,color_FFFFFF,t_70,g_se,x_16-1640877993761204.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/34849cc080e8401bb1f1d803b3d6f1bb.png)

## BRDF的属性

1. 非负性
   ![在这里插入图片描述](UE4ComputerGraphics.assets/eed23cd2209a49edb1a86a372a9b1396.png)
2. 线性
   ![在这里插入图片描述](UE4ComputerGraphics.assets/1601f66e7d6a405cbfca8ba9b1e79e39.png)
3. 可逆性
   ![在这里插入图片描述](UE4ComputerGraphics.assets/15f453d238c74e458c7a84a7f91a6999.png)
4. 能量守恒
   ![在这里插入图片描述](UE4ComputerGraphics.assets/ddf85ab228af48f1be2bf14ac7587a90.png)
5. 各向同性
   ![在这里插入图片描述](UE4ComputerGraphics.assets/d7cb4f6f27ad40889779976b7dd33592.png)

# 先进的光线传输理论

## BDPT(双向路径追踪)：从光源和相机分别发出光线，路径末端相连形成通路

![在这里插入图片描述](UE4ComputerGraphics.assets/bea49f3ac0d04b81a8fdcbacd4039627.png)

## MLT：马尔科夫链帮助采样：

- 马尔可夫链：根据当前样本，根据一个概率分布，生成下一个相近的样本
- 可以做到以任意函数为pdf生成样本
- 有一个路径的情况下，可以生成相似的路径
- 有了种子，就能找到更多相似的

## 光子映射

步骤：

1. 从光源发出光子，最终停在漫反射表面上
2. 从相机发出光线，最终和光源的光子碰撞
3. 局部光子密度估计:光子密度=光子数量/面积
   光子数量少：面积大，噪声大；光子数量大：模糊
   光子密度估计在数量趋向无限时才与真正光子密度相等

## VCM双向路径追踪和光子映射

用光子映射利用双向路径追踪的浪费问题，产生附近的光子
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_16,color_FFFFFF,t_70,g_se,x_16-1640877993762205.png)

## IR即时光线

光打到的地方作为新的光源，像素点时所有新光源的光线的累加
![在这里插入图片描述](UE4ComputerGraphics.assets/efa99bff808243679f0daff7d98edf36.png)

# 先进的外观建模

## 散射介质

云，雾
光照到这种介质上一部分吸收一部分反射
![在这里插入图片描述](UE4ComputerGraphics.assets/766a4e0ba1a444f29dc0045594261617.png)

## 人的头发

Kajiya_Kay Model
Marschner Model（效果好）

## 毛皮

动物的毛发髓质大，散射厉害。不适合用头发模型
Double Cylinder Model——Lobes

## 粒子

## 表面模型

次表面反射
（BSSRDF）
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993762206.png)
（Dipole Approximation）
用两个光源模拟次表面散射
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993762207.png)

## 布料

按表面渲染，按纤维渲染
计算量恐怖

## 多细节材质

## 微表面BRDF

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16-1640877993762208.png)

## 波动性

波动光学计算BRDF

- 结果与几何光学类似，但不连续（干涉导致），不同波长也不一样

## 程序化材质

houdini

# 相机，视场

## 相机

相机的原理：小孔成像
缺点：无虚化，全部清晰

## 视场（FOV）

![在这里插入图片描述](UE4ComputerGraphics.assets/9094017bb76c443ea0c9988621c588e4.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/8ffe1b5cdb0548c1a7ef9dcb4ac6ea9f.png)

- h: 传感器高度
- f: 焦距，传感器与透镜的距离
  以f=35mm为基准
  f=17mm 是广角镜头

## 曝光

H = T * E
Exposure = time x irradiance
快门和光圈分别控制曝光时间和光线的量

曝光等级（F_Number）（光圈大小）：数字越小，光圈越大 F_Number= f/d
![在这里插入图片描述](UE4ComputerGraphics.assets/8979550f6b5b49ba94f89b98807a067f.png)

快门速度过慢产生运动模糊效果
光圈过大有浅景深的效果

## 感光度（ISO）

线性：iso 200 需要 iso 100 一半的光

## 噪声

产生原因：光子数量少
影响因素：光圈过大，快门太慢，感光度太大
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_19,color_FFFFFF,t_70,g_se,x_16-1640877993762209.png)

## 高斯薄透镜方程

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993762210.png)
通过透镜组之间的相对位置变化来模拟焦距的改变

## 焦散模糊光线追踪

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_16,color_FFFFFF,t_70,g_se,x_16-1640877993762211.png)

对于x '每个像素的传感器（胶片）
在透镜平面上抽样随机点x”
通过镜头的光线会击中x ’ '(因为x '是焦点，考虑虚拟光线(x '，镜头中心))
估计辐射射线x " -> x " ’

## 景深（FYI）

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993763212.png)

光圈变化影像模糊的范围
但焦平面一定不会模糊
当模糊圈（CoC）小到一定程度，认为是清晰的，附近清晰的距离长度称为景深
光圈越小，景深范围越大
焦距越大，景深范围越小

## 光场

记录了空间中光线的集合，包含光线在空间中的位置和角度信息
普通相机体现不了远近信息，而光场相机可以先拍照后调焦

# 颜色

颜色是人的感知，而非光的性质

## 加色系统

所有颜色都是红绿蓝三色叠加形成的
![在这里插入图片描述](UE4ComputerGraphics.assets/6d57331f82cc4015870cb787c4fa9194.png)

## 色域

色域就是指某种表色模式所能表达的颜色构成的范围区域
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_19,color_FFFFFF,t_70,g_se,x_16-1640877993763213.png)

## 色调(Hue)，饱和度(Saturation)，亮度(Value)

![在这里插入图片描述](UE4ComputerGraphics.assets/4557c666dff9473eaa6392e40c981bb6.png)

## 互补色

黑白
红绿
黄蓝

## 减色系统

白色->黑色
添加各种颜色越混越黑

# 动画

## 关键帧动画

技术要点：动作之间插值补帧，使动作流畅
Interpolation 插值

## 物理模拟

模拟、仿真：推导、实现公式，模拟出物体应该怎么变化
例子：布料模拟、流体模拟

## 质点弹簧系统

特点：没有初始长度，随着拉力线性增长
![在这里插入图片描述](UE4ComputerGraphics.assets/1398f26a819c4b11b28adacca3ec2401.png)
表示方法：
![在这里插入图片描述](UE4ComputerGraphics.assets/71bf1fd8de834f3696e2de9c4fdb8687.png)

## 摩擦力

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16.png)

## 弹簧结构

多个点之间添加拉力模拟布料
![在这里插入图片描述](UE4ComputerGraphics.assets/2582bcf5e931415b8ad78cdf804529d1.png)

## 粒子系统

粒子单独有自己的属性，群体中产生新的属性

## 正向运动学

明确骨骼之间的运动关系→计算出各个部位的位置
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_18,color_FFFFFF,t_70,g_se,x_16-1640877993763214.png)

## 逆向运动学

限制各个部位（通常只有终端）的位置、限制骨骼的运动方式→计算骨骼的运动
方便控制形体整体形状
解特别复杂，可能并不唯一
解法：随机化算法（优化方法，梯度下降）
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_19,color_FFFFFF,t_70,g_se,x_16.png)

## 绑定

对形体的控制，像牵线木偶一样
Blend Shapes 控制点间的位置插值计算

## 动作捕捉

真人控制点反映到虚拟角色中去，需要建立真实和虚拟的联系

# 粒子系统的模拟

## 常微分方程

建立场的概念
![在这里插入图片描述](UE4ComputerGraphics.assets/00f239e55eb744daa34ba86fa743c4f5.png)

## 欧拉法

如果下一步和上一步的计算相差很远，就减小步长（Δt）来增加精确度
![在这里插入图片描述](UE4ComputerGraphics.assets/27a34e6a0c50429ca1d28a81021ecd1c.png)
![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_8,color_FFFFFF,t_70,g_se,x_16.png)

## 解决误差的方法

1. 用两次欧拉
   ![在这里插入图片描述](UE4ComputerGraphics.assets/a6db37c204dc4ce7a057b9ad838761d6.png)
2. 自适应步长
   分半效果好就不再分

## 数值分析方法，加速下降步长

牛顿法，龙格库塔。。

## 刚体模拟

![在这里插入图片描述](UE4ComputerGraphics.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDkzMjM1MzM=,size_20,color_FFFFFF,t_70,g_se,x_16.png)

## 流体模拟

假设：水是由小球组成的，水不可压缩和拉伸，一个小球位置的变化对其周围密度的影响，密度有变化，就要想着改回去，移动小球位置

流体模拟中两种不同的思路：Eulerian vs. Lagrangian

Lagrangian：质点法，以每个元素为单位模拟
Eulerian：网格法，以空间为单位分割模拟

## MPM

物质由粒子组成，粒子有属性

### 例题

> 连接绳子的约束
> 在 rope.cpp 中, 实现 Rope 类的构造函数。这个构造函数应该可以创建一个
> 新的绳子 (Rope) 对象，该对象从 start 开始，end 结束，包含 num_nodes 个节点。
> 也就是如下图所示：
> 每个结点都有质量，称为质点；质点之间的线段是一个弹簧。通过创建一系列的
> 质点和弹簧，你就可以创建一个像弹簧一样运动的物体。
> pinned_nodes 设置结点的索引。这些索引对应结点的固定属性 (pinned attribute) 应该设置为真（他们是静止的）。对于每一个结点，你应该构造一个 Mass
> 对象，并在 Mass 对象的构造函数里设置质量和固定属性。（请仔细阅读代码，确
> 定传递给构造函数的参数）。你应该在连续的两个结点之间创建一个弹簧，设置弹
> 簧两端的结点索引和弹簧系数 k，请检查构造函数的签名以确定传入的参数。
> 运行./ropesim。你应该可以看到屏幕上画出绳子，但它不发生运动。

```
Rope::Rope(Vector2D start, Vector2D end, int num_nodes, float node_mass, float k, vector<int> pinned_nodes)
{
    Vector2D node_distance = (end-start)/(num_nodes+1);

    for (int i= 0; i <num_nodes+2; i++){
        Mass *node = new Mass(start+node_distance*i, node_mass, false);
        masses.push_back(node);
        if(i ==0) continue;
        Spring* spring =new Spring(masses[i-1], masses[i], k);
        springs.push_back(spring);
    }
    for (auto &i : pinned_nodes) {
        masses[i]->pinned = true;//锁住绳子的一些点
    }
}

void Rope::simulateEuler(float delta_t, Vector2D gravity)
{
    for (auto &s : springs)
    {
        Vector2D v2_ba = s->m2->position - s->m1->position;
        double dis = v2_ba.norm();
        Vector2D dir_ba = v2_ba / dis;
        Vector2D f_ab = s->k
                * dir_ba
                * (dis  - s->rest_length);
        Vector2D f_ba = -f_ab;

        s->m1->forces +=f_ab;
        s->m2->forces +=f_ba;
    }
    for (auto &m : masses)
    {
        if (!m->pinned)
        {
            Vector2D a = m->forces /m->mass + gravity;
            m->velocity += a *delta_t;
            //m->position += m->velocity *delta_t;//显示欧拉
            Vector2D v_new = m->velocity + a * delta_t;
            m->position += v_new * delta_t; //半隐式
        }
        m->forces = Vector2D(0, 0);
    }
}

void Rope::simulateVerlet(float delta_t, Vector2D gravity)
{
    for (auto &s : springs)
    {
        Vector2D v2_ba = s->m2->position - s->m1->position;
        double dis = v2_ba.norm();
        Vector2D dir_ba = v2_ba / dis;

        if (s->m1->pinned && s->m2->pinned) continue;
        if ( s->m2->pinned && !s->m1->pinned){//m1 不是固定点 m2是
            s->m1->position += dir_ba *(dis  - s->rest_length) ;
        }else if( s->m1->pinned && !s->m2->pinned){//m1是固定点 m2不是
            s->m2->position += -dir_ba *(dis  - s->rest_length);
        }else {//m1m2不是
            s->m1->position += dir_ba *(dis  - s->rest_length) * 0.5;
            s->m2->position += -dir_ba *(dis  - s->rest_length) * 0.5;
        }
    }
    for (auto &m : masses)
    {
        if (!m->pinned)
        {
            Vector2D a = m->forces /m->mass + gravity;
            Vector2D v_old = m->position;
            double damping = 0.00005;
            //显式 Verlet add damping
            m->position += (1-damping) * (m->position - m->last_position)  + a *delta_t*delta_t;
            //显式 Verlet
            //m->position += m->position - m->last_position + a *delta_t*delta_t;
            m->last_position = v_old;
        }
    }
} 
```

最终作业：
https://s3.us-west-2.amazonaws.com/secure.notion-static.com/679ea083-8000-46ed-91a7-df06151e5a67/GAMES101_Final_Project.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211130T122027Z&X-Amz-Expires=86400&X-Amz-Signature=59990be38d7763a0c77893b64d73c2c9d19eb260f8fccae0e23d4370354772d0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22GAMES101_Final_Project.pdf%22&x-id=GetObject

贴个链接，直接跑路。