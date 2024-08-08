# Opencv的100道题目
## 1-10题的题目：
### 1-20需要引入头文件：A_1_10.h
- [通道交换](/A1.cpp "通道交换")

- [灰度化](/A2.cpp "灰度化")
- [二值化](/A3.cpp "二值化")
- [大津二值化算法](/A4.cpp "大津二值化算法")
- [RGB->HSV色域变换](/A5.cpp "RGB->HSV色域变换")
- [减色处理](/A6.cpp "减色处理")
- [平均池化](/A7.cpp "平均池化")
- [最大池化](/A8.cpp "最大池化")
- [高斯滤波](/A9.cpp "高斯滤波")
- [中值滤波](/A10.cpp "中值滤波")

## 1-10题重要算法解析：
### 一维高斯分布
<img src="/images/一维高斯分布.png" alt="GitHub set up" style="zoom:50%;" />

### 二维高斯分布
<img src="/images/二维高斯分布.png" alt="GitHub set up" style="zoom:50%;" />

### 大津二值化算法--最大类内方差
<img src="/images/最大类内方差1.png" alt="GitHub set up" style="zoom: 67%;" />
<img src="/images/最大类内方差2.png" alt="GitHub set up" style="zoom:67%;" />

> 大津二值化算法的要点：
>
> 1. 大津二值化算法的算法假设：图像灰度分布呈现双峰性，同时类间方差与图像分割的质量具有较大相关性；
>    1. 大津算法假设，如果能够找到一个阈值 t，使得分割后的两个类别（C0 和 C1）的类间方差  \sigma_B^2(t) *σ**B*2(*t*) 达到最大值，那么这个阈值 t 将能够最有效地将图像分成两部分，使得前景和背景之间的差异最大化；
>    2. 存在两个明显的峰值，一个代表前景（目标），另一个代表背景；
> 2. 在图像灰度分布呈现双峰时，为什么类间方差最大能够很好地分割图像呢？
>    1. 如果图像灰度呈现双峰，那么前景和背景的平均灰度值之间的差异较为明显，而从数学上，方差表达的是数据集中每个样本点与其均值之间的差异程度；而大津二值化算法中如果A类像素的平均灰度值与图像整体平均灰度值的差异程度，和 B类像素的平均灰度值与图像整体平均灰度值的差异程度的加和，最大，就此时的分割值能带来最好的分割效果。
> 3. ![image-20240803220436871](README.assets/image-20240803220436871.png)



### HSV色调翻转

> **HSV色调翻转：**
>
> 1. HSV图像的色调翻转是指将色调通道H，进行反转；其取值为0~180；因此翻转：180-H也是可以的；
> 2. fmod函数用于计算两个浮点数的余数；fmod(imgHSV.at<Vec3f>(y, x)[0] + 180, 360);
>
> **RGB图像转换为HSV图像：**
>
> 1. 该转换过程遵循固定的数据公式和计算流程：RGB通道归一化、先计算亮度V，再计算饱和度S，然后计算色调H；
>
> **HSV图像转换为RGB图像：**
>
> 1. 根据饱和度和亮度计算颜色的强度C、根据色调H将色彩图360度分成六个部分，每一部分对应一个RGB颜色的分量变化、根据色调分区，计算RGB分量的值；





### **减色处理**

> 1. 设定一个减色因子；原始数据遵循一个公式变换成新数据：原始像素 / n * n + n / 2; 





### 平均池化

> 1. 平均池化是将一张图像平均切分为N份网格；
> 1. 池化范围即卷积核尺寸，步长默认是与池化范围保持一致；但是也可以不一致；
> 1. 事实上，在很多经典的模型中，池化层的步长和池化范围是不一致的，这样一是为了降低空间分辨率，二是获取到更大空间区域的特征信息；



### 高斯滤波

> 1. 卷积操作，在工程实现上，卷积核是通过一行向量存在的，而卷积操作，相同读取顺序（先行后列还是先列后行）的卷积核和原始图像的卷积区域，进行乘积和相加；






## 11-10题的题目：
- [均值滤波器](/A11.cpp "均值滤波器")

> 1. 区别于池化层，它的步长是1；
> 2. 先定位到某个像素，根据卷积核的size计算对应区域的像素与卷积核的加权和。

- [Motion Filter](/A12.cpp "Motion Filter")

> 1. 与均值滤波器不同的是，后者的卷积核设计不同；
> 2. 卷积操作最需要注意的是边界的控制；
> 3. 卷积核所有权重的加和是需要等于1的；

- [MAX-MIN滤波器](/A13.cpp "MAX-MIN滤波器")

> 1. 虽然不是一个滤波核，但是**更为本质的是，卷积核代表了一种当前区域像素的计算规则**。

- [差分滤波器](/A14.cpp "差分滤波器")

> 1. 卷积核，在当前的工程环境下，似乎二维数组也是很合理的；
> 2. 差分滤波器，用于**检测覆盖区域内像素值的变化程度**；
> 3. 以水平差分滤波器为例，在当前工程实现中-1，1，0，这种搭配，配合uchar的数据范围（0~255），只能展现低像素向高像素的变化；而高像素向低像素的变化会呈现0；

- [Sobel滤波器](/A15.cpp "Sobel滤波器")

>1. 水平卷积核：不仅仅是2向-2的变化；而且还有
>
>2. ❓ 2与-2的卷积核 与 -2与2的卷积核的效果差别如何？—— 效果上还真有差别，但是具体的差别不太懂(从图像上看是黑白的差别)
>
>   <img src="README.assets/image-20240805004433394.png" alt="image-20240805004433394" style="zoom:50%;" /><img src="README.assets/image-20240805004421493.png" alt="image-20240805004421493" style="zoom:50%;" /> 
>
>   

- [Prewitt滤波器](/A16.cpp "Prewitt滤波器")

> 1. 两个卷积核
>
>    ```
>    	double kPrewittHorizontal[kSize][kSize] = { {-1, 0, 1}, {-1, 0, 1}, {-1, 0, 1} };
>    	double kPrewittVertical[kSize][kSize] = { {-1, -1, -1}, {0, 0, 0}, {1, 1, 1} };
>    ```
>
> 2. 目前来看，综合soble和当前核，一般左边或者上边的权重都是负值；

- [Laplacian滤波器](/A17.cpp "Laplacian滤波器")

> 1. double kLaplacian[kSize][kSize] = { {0, 1, 0}, {1, -4, 1}, {0, 1, 0} }; // 特殊的设计；

- [Emboss滤波器](/A18.cpp "Emboss滤波器")

> 1. double kEmboss[kSize][kSize] = { {-2, -1, 0}, {-1, 1, 1}, {0, 1, 2} };
> 2. 模仿光照方向，呈现立体感和浮雕的感觉；

- [LoG滤波器](/A19.cpp "LoG滤波器")

> 1. LoG滤波器是一种结合了高斯平滑和拉普拉斯操作的滤波器，用于检测图像中的边缘和斑点（blob）。

- [直方图(C++)](/A20.cpp "直方图")
- [直方图(python)](/A20.py "直方图")

### 11-20需要引入头文件：A_11_20.h
11-20题全是关于滤波器的题目

### 什么是边缘？
边缘一般是指图像中某一局部强度剧烈变化的区域。强度变化一般有2中情况，阶跃效应和屋顶效应。而边缘检测的任务就是找到具有阶跃变化或者屋顶变化的像素点的集合。
边缘检测基本原理：
- 对于阶跃效应：一阶微分的峰值为边缘点，二阶微分的零点为边缘点。
具体来说，从阶跃效应的那张图可以看到边缘处的斜率（一阶导）最大，所以一阶微分的峰值是边缘点，而斜率是先增大后减小的，即边缘点的二阶导为0处。
#### 阶跃效应
![GitHub set up](/images/阶跃效应.png)

- 对于屋顶效应：一阶微分的零点为边缘点，二阶微分的峰值为边缘点。
#### 屋顶效应
![GitHub set up](/images/屋顶效应.png)


## 11-20题重要算法解析：
### 均值滤波器
![GitHub set up](/images/均值滤波卷积核.png)
### 运动滤波器
![GitHub set up](/images/运动滤波器.png)
### MAX-MIN滤波器(效果图)
![GitHub set up](/images/max-min.png)
### 差分滤波器
![GitHub set up](/images/垂直滤波器.png)

![GitHub set up](/images/水平滤波器.png)

### Sobel滤波器
![GitHub set up](/images/Sobel卷积核.png)
### Prewitt滤波器
![GitHub set up](/images/prewitt算子.png)
### Laplacian滤波器
![GitHub set up](/images/4领域拉普拉斯算子.png)
### Emboss滤波器
![GitHub set up](/images/Emboss算子.png)
### LoG滤波器
![GitHub set up](/images/高斯拉普拉斯算子.png)
### 直方图
<img src="/images/直方图.png" alt="GitHub set up" style="zoom:50%;" />

## 21-30题的题目：
### 11-20需要引入头文件：A_21_30.h
- [直方图归一化](/A21.cpp "直方图归一化")

> 1. 当前的工程实现中，归一化是在调整图像像素的取值范围，从非0非255的区间调整到0~255的区间。最为关键的操作是：imgOut.at<Vec3b>(y, x)[c] = (uchar)((newEnd-newStart)/(end-start)*(val-start)+newStart);

- [方图操作](/A22.cpp "方图操作")

> 1. 具体的操作目的不清楚。应该是希望调整图像的正态分布；

- [直方图均衡化](/A23.cpp "直方图均衡化")

> 1. double hist[255];` 这里声明了一个长度为255的数组，但实际上应该是256，因为灰度级范围是从0到255，共256个可能的值。

- [伽玛校正](/A24.cpp "伽玛校正")

> 1. 图像的像素矫正，是通过统计图像整体或者局部的信息，来调整每个位置的像素值。
> 2. 伽马矫正的时候，是基于0~1区间的像素值进行的；

- [最邻近插值](/A25.cpp "最邻近插值")

> 1. 工程实现中，输出图像每个位置的像素，也都是与输入图像每个位置的像素是一一对应的；
> 2. 原理：当图像需要放大或者缩小时，会按照缩放比例找到离目标位置最近的已知像素，并将值赋值给目标位置；该思路在工程实现上是通过static_cast<int>截断的方式控制的；

- [双线性插值](/A26.cpp "双线性插值")

> 1. 双线性插值，某一个位置的像素是基于从其当前位置（的floor+int形式）开始的斜向下的四个点的坐标，以及其他三个点到起始点的距离的倒数作为权重，进行加权和得到的。
> 2. 相比于最邻近插值，它考虑了更多相邻点的像素且权重计算方式比较合理，因而它的平滑性更好；

- [双三次插值](/A27.cpp "双三次插值")

> 1. 它考虑了更多的相邻点，16个；其平滑性更好，细节保持能力更高，对应地，它的计算复杂度比较高；
> 2. 权重计算同样基于目标位置与相邻点之间距离的倒数；
> 3. 旋转的时候，可以考虑使用双三次插值，能获得更好的效果；
> 4. 16个相邻点，同样是基于目标位置的floor+int形式，进行寻找；
> 5. **这里使用到了三次插值函数❓**

- [仿射变换-平行移动](/A28.cpp "仿射变换-平行移动")

> 1. **单个像素级别的平行移动与放缩，依赖于公式，但是理解公式才是重点；**

- [仿射变换-放大缩小](/A29.cpp "仿射变换-放大缩小")
- [仿射变换-旋转](/A30.cpp "仿射变换-旋转")

## 21-30题重要算法解析：
### 均值方差均方差
![GitHub set up](/images/均值方差标准差.jpg)
### 伽马矫正
![GitHub set up](/images/伽马矫正.png)
### 最邻近插值
![GitHub set up](/images/最邻近插值.png)
### 双线性插值
![GitHub set up](/images/双线性插值1.png)
![GitHub set up](/images/双线性插值2.png)
![GitHub set up](/images/双线性插值4.png)
![GitHub set up](/images/双线性插值3.png)
### BiCubic函数
![GitHub set up](/images/BiCubic函数.png)
### 双三次插值
![GitHub set up](/images/双三次插值2.png)
![GitHub set up](/images/双三次插值1.png)
![GitHub set up](/images/双三次插值5.png)
### 齐次坐标系到笛卡尔坐标系转换
![GitHub set up](/images/齐次到笛卡尔坐标.png)
### 放射变换
![GitHub set up](/images/仿射变换1.png)
![GitHub set up](/images/仿射变换2.png)
![GitHub set up](/images/仿射变换3.png)
#### 平移量为0的仿射变换
![GitHub set up](/images/平移量为0的仿射变换.png)  
![GitHub set up](/images/仿射变换4.png)  
![GitHub set up](/images/放射变换5.png)
## 31-40题的题目：
### 31-40需要引入头文件：A_31_40.h
- [仿射变换（Afine Transformations）——倾斜](/A31.cpp "仿射变换（Afine Transformations）——倾斜")

> 

- [傅立叶变换](/A32.cpp "傅立叶变换")

>1. 傅里叶变换中，低频表示图像中变化比较缓慢的区域，而高频表示图像中变化比较剧烈的区域；

- [傅立叶变换——低通滤波](/A33.cpp "傅立叶变换——低通滤波")
- [傅立叶变换——高通滤波](/A34.cpp "傅立叶变换——高通滤波")
- [35 傅立叶变换——带通滤波](/A35.cpp "傅立叶变换——带通滤波")

> 1. **傅里叶表述中，最为神奇的一点是：某个位置的频率分量（的幅度）表示了图像在不同频率下的贡献；它强调的是图像在某个频率下的贡献度，是一种贡献，而且主体是图像，而不是对应位置的单个像素**。
>
> 2. *F*(*u*,*v*)=*A*(*u*,*v*)+*j**B*(*u*,*v*) 其中A表示实部，B表示虚部；
>
> 3. **某个位置的频率分量的振幅和相位信息是如何根据对应位置的像素值和整个图像的像素计算的呢**
>
>    **<u>某一位置的像素值、坐标和整张图像的像素和位置信息 --> 计算频率分量的实部和虚部 --> 计算振幅和相位；</u>** 🎯
>
>    ![image-20240806222033078](README.assets/image-20240806222033078.png)
>
> 4. 低通滤波的实现方式：**对称性的过滤过程！**
>
>    ![image-20240806223906614](README.assets/image-20240806223906614.png)

- [36 霍夫变换：直线检测-霍夫逆变换](/A36.cpp "霍夫变换：直线检测-霍夫逆变换")

> 

- [37 离散余弦变换](/A37.cpp "离散余弦变换")

> 1. 它将一组离散的数据序列转换为一组离散的余弦系数，特点是，将数据转换成分解在不同频率成分上的能量。**它类似于傅里叶变化，因此转换得到的一组离散余弦系数，表示立刻原始数据在不同频率上的贡献**，它更适合于实数信号和有限长度的数据。

- [38 离散余弦变换+量化](/A38.cpp "离散余弦变换+量化")

>1. 量化，对离散余弦系数进行近似表示，通过舍入或者**映射到更小的范围（量化的本质）**来减少数据量。
>2. JPEG压缩中使用的是离散余弦变换，而不是傅里叶变换。原因在于：1. **实用性：它生成的是实数系数，而不是复数系数**；2. 计算效率较高；3. 能量集中性，DCT能将大部分图像的能量集中在较少的低频成分上，这与人类视觉系统的特征相符：即更倾向于关注图像的整体结构和主要特性，即图像中变化缓慢的部分。”**在JPEG压缩中，由于人眼对低频信息更为敏感，DCT能够将图像的大部分能量集中在低频成分上。这样做可以保留图像的主要结构和整体特征，同时通过量化和丢弃高频成分来实现有效的数据压缩，而这种处理方式在视觉上对图像的影响相对较小。**“

- [39 YCbCr 色彩空间](/A39.cpp "YCbCr 色彩空间")

> 1. YCbCr 色彩空间,对于视频编码和图像压缩标准十分重要。
> 2. 亮度Y；色度CbCr。这种分离使得在处理图像时能够更加有效地处理亮度和色度信息，**因为它们通常具有不同的空间频率特性**。
> 3. 人眼对亮度（Y）信息更为敏感，而对色度（CbCr）信息的感知较低。因此，在图像和视频压缩中，可以更多地保留Y分量的精度，而对Cb和Cr分量进行更高程度的压缩，以减少数据量而对图像质量影响较小。
> 4. YCbCr允许不同的色度抽样方式，如4:4:4、4:2:2和4:2:0。这些抽样方式决定了每个Y分量采样点相对于Cb和Cr分量的数量，影响了图像或视频的色彩保真度和压缩效率。
> 5. 在图像和视频压缩标准中，如JPEG和MPEG，YCbCr色彩空间的使用能够显著提高压缩效率。因为在这些标准中，通常会对Cb和Cr分量进行更强的压缩，而Y分量则保留相对较高的精度，从而实现更高的压缩比而保持可接受的视觉质量。
> 6. **总的来说，图像转换到YCbCr空间，基于三个分量对于人类感知的感知差异 --> 可以通过调整三个分量的比例，以及对三个分量不同的量化程度/压缩程度 --> 实现整体更高质量的压缩比。**

- [40 YCbCr+离散余弦变换+量化](/A40.cpp "YCbCr+离散余弦变换+量化")

## 31-40题重要算法解析：
### 傅里叶级数
![GitHub set up](/images/傅里叶级数1.png)
![GitHub set up](/images/傅里叶级数2.png)
### 欧拉公式
![GitHub set up](/images/欧拉公式.png)
### 傅里叶变换
![GitHub set up](/images/傅里叶变换2.png)
![GitHub set up](/images/傅里叶变换1.png)
### 二维离散余弦变换
![GitHub set up](/images/二维离散余弦变换1.png)
### 均方误差
![GitHub set up](/images/均方误差.png)
### 均方误差
![GitHub set up](/images/均方误差.png)
### YCbCr色彩空间
![GitHub set up](/images/YCbCr_1.png)
![GitHub set up](/images/YCbCr_2.png)
![GitHub set up](/images/YCbCr_3.png)

## 41-50题的题目：
### 41-50需要引入头文件：A_41_50.h
- [Canny边缘检测：边缘强度](/A41.cpp "Canny边缘检测：边缘强度")

> 1. sobel算子是一种边缘检测的算子，计算图像每个像素的一阶导数/梯度；
> 2. canny边缘检测则是一种更复杂的边缘检测方法，它使用多个步骤来优化边缘检测的结果，包括使用sobel梯度信息、非极大值抑制和双阈值检测等。
> 3. 边缘梯度信息的工程实现中：灰度-->高斯滤波-->计算水平梯度图和垂直梯度图(**一点的梯度是由周围6个相邻点像素和权重值决定的**)。
> 4. 整体方向的梯度图，需要注意使用uchar来截断超出范围的像素值。
> 5. 整体方向的角度图，结合梯度值可以计算出角度，根据需要可以将角度映射到有限个角度值。

- [Canny边缘检测：边缘细化](/A42.cpp "Canny边缘检测：边缘细化")

> 1. 非极大值抑制：在众多梯度较大的区域中，寻找到真实的边缘。基于一个假设：边缘是局部梯度值最大；
> 2. 之所以是选择梯度法线方向上的两个像素是，因为在当前像素的局部范围内，当前像素与梯度方向上的两个像素的梯度值比较是没有意义的。
> 3. **从效果上看，也没有感受到边缘有多么明显**

- [Canny 边缘检测：滞后阈值](/A43.cpp "Canny 边缘检测：滞后阈值")

> 1. 它基于边缘的连接性和强边缘的特性，减少假阳性的边缘检测结果！（梯度较小的弱边缘像素和边缘附近被强行连接的弱边缘像素）
> 2. 两个阈值能够剔除掉一部分的梯度较小的虚假边缘像素；而“当前像素附近是否存在强边缘像素”的规则将保留那些梯度较小的真实边缘像素；后者也被认为是边缘跟踪。
> 3. **边缘检测需要一个合适的应用题❓**

- [44 霍夫变换：直线检测](/A44.cpp "霍夫变换：直线检测")

> 1. 44的堆问题已经解决，问题出在了hough_table上；
> 2. 使用霍夫变换的效果不好，可能需要调整参数❓

- [霍夫变换：直线检测-NMS](/A45.cpp "霍夫变换：直线检测-NMS")

> 1. 非极大值抑制的本质：一个相邻区域中，给与最符合规则（**专业词汇：响应值**）的元素最大的重视，而对该区域中的其他元素进行剔除；
> 2. 更为专业的说法：非极大值抑制的核心思想是在**局部范围内**，选择响应值最大的点，并**抑制其他不符合条件的响应**。
> 3. 45cpp需要改进，暂时不去考虑优化

- [霍夫变换：直线检测-霍夫逆变换](/A46.cpp "霍夫变换：直线检测-霍夫逆变换")
- [膨胀（Dilate）](/A47.cpp "膨胀（Dilate）")

> 1. 在100Q的工程实现中，使用的核是MORPH_CROSS，所以找到的元素是当前元素的上下左右元素；如果是MORPH_RECT，则是需要遍历kernel的行和列；
> 2. 同样在100Q的工程实现中，它是以输出图像中的每个像素进行遍历；而另一种方式是以输入图像的每个像素进行遍历；似乎前者更为合理和复杂度低些；**//对于待操作的像素(x,y)=0，(x, y-1)，(x-1, y)，(x+1, y)，(x, y+1)中不论哪一个为255，令(x,y)=255。**

- [腐蚀（Erode）](/A48.cpp "腐蚀（Erode）")

> //对于待操作的像素(x,y)=0，(x, y-1)，(x-1, y)，(x+1, y)，(x, y+1)中不论哪一个不为255，令(x,y)=0。

- [开运算（Opening Operation）](/A49.cpp "开运算（Opening Operation）")

- [闭运算（Closing Operation）](/A50.cpp "闭运算（Closing Operation）")

## 41-50题重要算法解析：
### Canny边缘检测-高斯核
![GitHub set up](/images/canny1.png)
### Canny边缘检测-Sobel核（垂直和水平）
![GitHub set up](/images/canny2.png)
### Canny边缘检测-求梯度
![GitHub set up](/images/canny3.png)
### 霍夫变换
#### 笛卡尔坐标系下直线
![GitHub set up](/images/霍夫变换1.png)  
#### 霍夫变换下坐标方程
![GitHub set up](/images/霍夫变换2.png)  
#### 笛卡尔坐标系->霍夫坐标系（直线）
![GitHub set up](/images/霍夫变换3.png)  
#### 笛卡尔坐标系->霍夫坐标系（点）
![GitHub set up](/images/霍夫变换4.png)  
#### 笛卡尔坐标系->霍夫坐标系（两点）
![GitHub set up](/images/霍夫变换5.png)  
#### 笛卡尔坐标系->霍夫坐标系（三点）
![GitHub set up](/images/霍夫变换6.png)  
#### 笛卡尔坐标系->霍夫坐标系（多点）
![GitHub set up](/images/霍夫变换7.png)  
#### 通过霍夫坐标系下找最好共线点
![GitHub set up](/images/霍夫变换8.png)  
#### 极坐标系
![GitHub set up](/images/霍夫变换9.png)  
#### 极坐标系->霍夫坐标系
![GitHub set up](/images/霍夫变换10.png)  
#### 伪代码
![GitHub set up](/images/霍夫变换11.png)  

## 51-60题的题目：
### 51-60题需要引入头文件：A_51_60.h
- [形态学梯度（Morphology Gradient）](/A51.cpp "形态学梯度（Morphology Gradient）")

> 1. 即膨胀图像减去腐蚀图像，核的大小需要视被处理主体的像素大小；

- [顶帽（Top Hat）](/A52.cpp "顶帽（Top Hat）")

> 1. 二值原始图像-开运算图像：突出展现的是，图像中消失的内容（完全被腐蚀掉的内容/**突出细小的亮结构**），被腐蚀后不能恢复的一定是 **独立的白色区域，且尺寸小于核的尺寸**

- [黑帽（Black Hat）](/A53.cpp "黑帽（Black Hat）")

> 1. 闭运算-原始图像，**突出比周围区域更暗的小结构**;

- [误差平方和](/A54.cpp "误差平方和")

>1. 误差平方和的应用场景有：图像匹配/模板匹配；特征匹配等；
>2. 100Q的工程实现中，**在原始图像的每个像素的位置，计算一次**模板与原始图像当前区域的SSD；

- [绝对值差和](/A55.cpp "绝对值差和")

> 1. 应用场景：模板匹配、运动分析(**通过比较相邻帧之间的像素差异，可以检测出视频中物体的运动或运动区域的变化**)、变化检测(**通过对比两幅时间序列的遥感图像，可以识别出例如城市扩展、农田变化或自然灾害影响下的地表变化**)；

- [归一化交叉相关](/A56.cpp "归一化交叉相关")

> 1. NCC通常用于模板匹配/相似性检测/物体检测和追踪；
>
>    ![image-20240808223306524](README.assets/image-20240808223306524.png)
>
>    

- [零均值归一化交叉相关](/A57.cpp "零均值归一化交叉相关")

> 1. 模板图像减去模板的像素均值；原始图像减去子图的像素均值；
> 2. ZNCC通过消除亮度和对比度变化的影响（光照不变性、对比度不变性），提高了匹配的准确性，尤其适用于要求匹配稳健性的应用场景。

- [4邻域连通域标记](/A58.cpp "4邻域连通域标记")

> 1. 应用于物体计数、分割；
>
> 2. **个人认为：**邻域连通域标记没有基于二值化的轮廓搜索好用，后者能提供轮廓位置信息，进而统计出更多的特征信息，用于筛选；
>
> 3. 通过将像素值标记为物体类别号，避免对所有像素进行遍历；
>
>    <img src="README.assets/image-20240808224702828.png" alt="image-20240808224702828" style="zoom:67%;" />
>
>    

- [8邻域连通域标记](/A59.cpp "8邻域连通域标记")
- [透明混合（Alpha Blending）(py)](/A60.py "透明混合（Alpha Blending）")

> ![image-20240808225121623](README.assets/image-20240808225121623.png)
>
> 1. **图像合成和叠加**

## 51-60题重要算法解析：
### 形态学梯度
![GitHub set up](/images/形态学梯度.png)
### 模式匹配-误差平方和(SSD)
![GitHub set up](/images/误差平方和(SSD).png)

### 模式匹配-绝对值差和
![GitHub set up](/images/绝对值差和.png)
### 模式匹配-归一化交叉相关
![GitHub set up](/images/归一化交叉相关.png)
### 模式匹配-零均归一化
![GitHub set up](/images/零均归一化.png)
### 模式匹配-ncc zncc
![GitHub set up](/images/ncc_zncc.png)

## 61-70题的题目：
### 61-70需要引入头文件：A_61_70.h
- [4连接数(c++)](/A61.cpp "4连接数")
- [4连接数(python)](/A61.py "4连接数")
- [8连接数(c++)](/A62.cpp "8连接数")
- [8连接数(python)](/A62.py "8连接数")
- [细化处理(c++)](/A63.cpp "细化处理")
> - 细化处理常使用迭代算法，其中最常见的包括 Zhang-Suen 算法和 Guo-Hall 算法。这些算法在每次迭代中检查像素周围的相邻像素，并根据特定的条件决定是否删除当前像素。通常，删除的条件包括保持连通性和边界形状不变。

- [细化处理(python)](/A63.py "细化处理")
- [Hilditch 细化算法(c++)](/A64.cpp "Hilditch 细化算法")
- [Hilditch 细化算法(python)](/A64.py "Hilditch 细化算法")
- [Zhang-Suen细化算法(c++)](/A65.cpp "Zhang-Suen细化算法")
- [Zhang-Suen细化算法(python)](/A65.py "Zhang-Suen细化算法")
- [HOG-梯度幅值・梯度方向(python)](/A66.py "HOG-梯度幅值・梯度方向")

>     1. 梯度就是简单的灰度值相减。
>         # x方向： g_x = I(x + 1, y) - I(x - 1, y)
>         # y方向： g_y = I(x, y + 1) - I(x, y - 1)
>     2. 量化梯度：将梯度方向进行N等分（映射）

- [HOG-梯度直方图(python)](/A67.py "HOG-梯度直方图")

> 1. 

- [HOG-直方图归一化(python)](/A68.py "HOG-直方图归一化")
- [HOG-可视化特征量(python)](/A69.py "HOG-可视化特征量")
- [色彩追踪（Color Tracking）(python)](/A70.py "色彩追踪（Color Tracking）")

> 1. 色彩追踪首先涉及选择一个或多个感兴趣的颜色作为追踪目标。这些颜色通常以RGB（红绿蓝）或HSV（色调、饱和度、亮度）色彩空间中的范围来定义。
> 2. 设定阈值，限制每帧图像中符合阈值范围的区域？❓

- [色彩追踪（Color Tracking）(c++)](/A70.cpp "色彩追踪（Color Tracking）")

## 61-70题重要算法解析：
### 4连通
![GitHub set up](/images/4连通.png) 
### 8连通
![GitHub set up](/images/8连通.png)  
### HOG
![GitHub set up](/images/HOG1.png)  
![GitHub set up](/images/图像梯度1.png)  
![GitHub set up](/images/图像梯度2.png)  
![GitHub set up](/images/9bin.png)  
![GitHub set up](/images/block.png)  

## 71-80题的题目：
### 71-80题需要引入头文件：A_71_80.h
- [掩膜（Masking）](/A71.cpp "掩膜（Masking）")
> 1. out.at<Vec3b>(y, x)[c] = img.at<Vec3b>(y, x)[c] * bin.at<uchar>(y, x);

- [掩膜（色彩追踪（Color Tracking）+形态学处理）](/A72.cpp "掩膜（色彩追踪（Color Tracking）+形态学处理）")
- [缩小和放大](/A73.cpp "缩小和放大")

> 1. **严格的数学计算和严谨的边界处理**

- [使用差分金字塔提取高频成分](/A74.cpp "使用差分金字塔提取高频成分")

> 1. 它基于高斯金字塔（Gaussian Pyramid）的概念，通过对图像的不同高斯模糊版本进行减法操作来实现；对相邻的两个高斯模糊图像进行像素级减法操作，得到差分图像（Difference of Gaussians）。
> 2. 差分金字塔的底层包含了原始图像中的**高频成分，即图像中边缘、细节等明显变化的区域**。**因为高斯模糊在较高尺度上能平滑掉图像的低频信息（例如背景**），所以通过相邻高斯模糊图像的**差分**可以突出图像中的细节和边缘。
> 3. 质疑❓：在100Q的工程实现中，相减的两张图像的size并不一致，但是只有一致或者相似，才能正确地提取对应位置的高频成分。

- [高斯金字塔（Gaussian Pyramid）](/A75.cpp "高斯金字塔（Gaussian Pyramid）")
- [显著图（Saliency Map）](/A76.cpp "显著图（Saliency Map）")

>1. 基于空域的方式：N组差分在某个位置的差值加和，但是是否需要注意会超过255？❓
>
>   ![image-20240808233203403](README.assets/image-20240808233203403.png)
>
>   2. 有意思的结果：
>
>      ![image-20240808235510014](README.assets/image-20240808235510014.png)
>
>      

- [Gabor 滤波器（Gabor Filter）](/A77.cpp "Gabor 滤波器（Gabor Filter）")

> 1. 结合了高斯函数和正弦函数，能够在空间域和频率域上有效地描述图像的局部特征。

- [旋转Gabor滤波器](/A78.cpp "旋转Gabor滤波器")
- [使用Gabor滤波器进行边缘检测](/A79.cpp "使用Gabor滤波器进行边缘检测")
- [使用Gabor滤波器进行特征提取](/A80.py "使用Gabor滤波器进行特征提取")  

## 71-80题重要算法解析：
#### 三角函数+高斯分布
![GitHub set up](/images/三角函数_高斯分布.png)   
### gabor滤波器
![GitHub set up](/images/gabor滤波器.png)  
### gabor实数
![GitHub set up](/images/gabor实数.png)  
### gabor虚数
![GitHub set up](/images/gabor虚数.png)  
### gabor复数
![GitHub set up](/images/gabor复数.png)  
![GitHub set up](/images/gabor1.png)  
![GitHub set up](/images/gabor3.png)  

## 81-90题的题目：
- [Hessian角点检测](/A81.cpp "Hessian角点检测")

> 1. 计算图像梯度-->计算图像各像素的自相关矩阵-->计算图像各像素的响应值（行列式-迹平方*系数）-->阈值化处理（通常做局部最大值检测或者非极大值抑制）
>
> 2. 响应函数设计的理念
>
>    ![image-20240809002648834](README.assets/image-20240809002648834.png)

- [Harris角点检测第一步：Sobel + Gausian）](/A82.cpp "Harris角点检测第一步：Sobel + Gausian")
- [Harris角点检测第二步：角点检测](/A83.cpp "Harris角点检测第二步：角点检测")
- [简单图像识别第一步：减色化+柱状图^3](/A84.cpp "简单图像识别第一步：减色化+柱状图^3")
- [简单图像识别第二步：判别类别](/A85.cpp "简单图像识别第二步：判别类别")
- [简单图像识别第三步：评估](/A86.cpp "简单图像识别第三步：评估")
- [简单图像识别第四步：k-NN](/A87.cpp "简单图像识别第四步：k-NN")
- [k-平均聚类算法 第一步：生成质心](/A88.cpp "k-平均聚类算法第一步：生成质心")
- [k-平均聚类算法 第二步：聚类](/A89.cpp "k-平均聚类算法第二步：聚类")
- [k-平均聚类算法 第三步：调整初期类别](/A90.py "k-平均聚类算法 第三步：调整初期类别")  

## 81-90题重要算法解析：
#### Hessian矩阵
![GitHub set up](/images/Hessian矩阵.png)   
### 高斯滤波+Hessian矩阵
![GitHub set up](/images/高斯滤波_Hessian矩阵.png)  
### 图像一阶导数
![GitHub set up](/images/图像一阶导数.png)  
### 图像二阶导数
![GitHub set up](/images/图像二阶导数.png)  
### result
![GitHub set up](/images/结果.png)  
### 距离函数
![GitHub set up](/images/距离函数.png)  
### KNN
![GitHub set up](/images/KNN.png)  
### kmeans_dis
![GitHub set up](/images/kmeans_dis.png) 
### 质心
![GitHub set up](/images/质心.png) 

## 91-100题的题目：
- [平均聚类算法-按颜色距离分类](/A91.cpp "平均聚类算法-按颜色距离分类")
- [平均聚类算法-减色处理](/A92.cpp "平均聚类算法-减色处理n")
- [计算IoU](/A93.cpp "计算IoU")

- [随机裁剪（Random Cropping）](/A94.cpp "随机裁剪（Random Cropping）")

- [深度学习（Deep Learning）](/A95.cpp "深度学习（Deep Learning）")

- [神经网络（Neural Network）第二步——训练](/A96.cpp "神经网络（Neural Network）第二步——训练")

- [简单物体检测第一步----滑动窗口（Sliding Window）+HOG](/A97.cpp "简单物体检测第一步----滑动窗口（Sliding Window）+HOG")

- [简单物体检测第二步——滑动窗口（Sliding Window）+ NN](/A98.cpp "简单物体检测第二步——滑动窗口（Sliding Window）+ NN")

- [简单物体检测第三步——非极大值抑制（Non-Maximum Suppression）](/A99.cpp "简单物体检测第三步——非极大值抑制（Non-Maximum Suppression）")

- [简单物体检测第四步——评估（Evaluation）：Precision、Recall、F-Score、mAP](/A100.py "简单物体检测第四步——评估（Evaluation）：Precision、Recall、F-Score、mAP")  

## 91-100题重要算法解析：
#### IOU
![GitHub set up](/images/IOU1.png)  
![GitHub set up](/images/IOU2.png)  
#### NMS：非极大值抑制
![GitHub set up](/images/NMS.png) 







## 大知识点汇总

### 傅里叶变换

>1. 如何使用图像的傅里叶变换形式进行低通滤波？
>2. 某个位置的频率分量的振幅和相位信息是如何根据对应位置的像素值和整个图像的像素计算的呢?
>3. 上述表述中提到了两种概念都涉及加权叠加，那么两种概念下的权重是如何呈现和计算的呢
>4. **这种叠加与机器学习中的混合高斯模型是不是类似？**
>5. 是否可以认为，一张图像在频率域中的表达是M*N个频率分量的叠加？
>6. 一个图像的DFT和单个位置的频率分量之间的关系是如何
>7. 在你上述的表述中，提到某个位置的频率分量反映了对应位置的像素与整个图像在频率域上的关系，那这种关系的具象化表达可以是什么呢？
>8. 在上述的像素值和频率分量之间的转换关系中，频率分量的计算需要除对应位置像素值以外的整张图像的像素值，那么是否可以认为某个位置的频率分量包含了整张图像的信息以及当前位置的像素值与整张图像所有像素之间的某种关系？
>9. 二维图像在某一个位置的像素和该图像的DFT形式同一位置的频率分量之间存在什么关系吗
>10. 如果一个位置的频率分量的频率比较高，是否表示对应位置的像素相对于相邻区域中的像素差异较大
>11. 在上述表述中，u表示图像在水平方向上的重复的频率，是不是可以理解成，图像某个位置的像素在对应行的像素中相似的频率
>12. 上述表述中的u和v表示什么意思呢
>13. DFT中的每个元素被成为频率分量是吗
>14. 如上述解答，复数中的实部表示图像对应位置像素相对于整张图像的振幅和相位信息，这样的表述合适吗
>15. 一张图像某个位置的元素是整数，而该图像对应位置的傅里叶变换元素是复数形式是吗
>16. 对于一个图像执行傅里叶变换，将获取到同样长宽的矩阵吗
>17. 如上述表述中，我很好奇某个频率分量的相位信息在图像中表示什么形象化的意思
>18. 如上述表述，表示在傅里叶变换的低通滤波应用中，实际上只使用了振幅信息，而没有用到相位信息
>19. 在上述表述中，图像频率域表示中的离散值是如何体现频率的分量对应的振幅和相位信息的？
>20. **在上述表述中，一张图像的频率域表示的某个离散值，它是如何体现图像中某个像素或者区域是缓慢变换还是快速变化的？**
>21. 是否能够通俗地解释傅里叶变换的公式，更加形象地解释
>22. 如果不适用opencv提供的dft函数，那么该如何实现呢
>23. 基于c++和opencv如何实现图像的傅里叶变换
>24. 傅里叶变换在图像处理中的作用到底多大？傅里叶变换因为将图像从人眼直观的空间域变换到抽象的频率域，我认为这影响了它的应用
>25. 傅里叶变换中的带通滤波是什么意思
>26. 图像傅里叶变换中的频率域中的频率是指什么？具象化的表达是什么呢
>27. **如上述解答，傅里叶变换将图像从空间域转换到频率域，而直方图也涉及统计图像各个像素的频率，那这两种方法有什么异同吗**
>28. 图像上进行傅里叶变换的作用
