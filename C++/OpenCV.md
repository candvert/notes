截至2025年1月，opencv的最新版本为4.11.0，opencv的4.0.0版本发布于2018年11月18日。
opencv中所有类和函数都放在cv命名空间里。
opencv会处理所有底层内存问题。（官方文档的描述：使用OpenCV的C++接口，您不需要考虑内存管理。）
异常都是cv::Exception的实例或其子类的实例，而cv::Exception是std::exception的子类，所以可以使用c++中的库处理。
opencv的实现是完全可重入的。
## 主要模块
```html
core                Core functionality
imgproc             Image Processing
imgcodecs           Image file reading and writing
highgui             High-level GUI
videoio             Video I/O
video               Video Analysis
calib3d             Camera Calibration and 3D Reconstruction
features2d          2D Features Framework
objdetect           Object Detection
dnn                 Deep Neural Network module
ml                  Machine Learning
flann               Clustering and Search in Multi-Dimensional Spaces
photo               Computational Photography
stitching           Images stitching
gapi                Graph API
```
## 示例
```cpp
#include <opencv2/opencv.hpp>
using namespace cv;
int main()
{
    Mat img = imread("D:/a.jpg");
    imshow("Display window", img);
    waitKey(0);
}
```
## 理论知识
![](images/opencv_8.png)
## Mat - 基本图像容器
```cpp
// 如果无法读取图像（由于文件丢失、权限不正确或格式不受支持/无效），该函数将返回一个空矩阵。
// 如果设置IMREAD_UNCHANGED，则按原样返回已加载的图像（带有alpha通道）
// 如果设置IMREAD_GRAYSCALE，则始终将图像转换为单通道灰度图像
// 如果设置IMREAD_COLOR_BGR或IMREAD_COLOR，则始终将图像转换为 3 通道 BGR 彩色图像
// 如果设置IMREAD_COLOR_RGB，则始终将图像转换为 3 通道 RGB 彩色图像
Mat cv::imread(const String& filename, int flags = IMREAD_COLOR_BGR);

Mat new_image = Mat::zeros(image.size(), image.type());    // 所有元素值为0

saturate_cast<uchar>(859);    // 对于uchar类型返回值范围为[0,255]，即saturate_cast<uchar>(859);返回255，saturate_cast<uchar>(-239);返回0，而saturate_cast<uchar>(212);返回212

// 创建一个窗口。如果已经存在同名的窗口，该函数不执行任何操作。
// 常用flags：WINDOW_AUTOSIZE、WINDOW_NORMAL、WINDOW_FULLSCREEN、WINDOW_KEEPRATIO
void cv::namedWindow(const String& winname, int flags = WINDOW_AUTOSIZE);

void cv::imshow(const String& winname, InputArray mat);

// delay表示延迟多少毫秒，值为0表示无限等待
// 返回值的用法：int k = waitKey(); if (k == 's') cout << "good" << endl;
int cv::waitKey(int delay = 0);

Mat类常用的函数：
	int channels() const;    // 返回通道的个数
	int depth() const;    // 返回矩阵元素的深度，比如CV_8U
	int type() const;    // 返回矩阵元素的类型，比如CV_8UC3

Mat类的所有公共属性：
	MatAllocator* allocator;
	int cols;
	uchar* data;
	const uchar* dataend;
	const unchar* datalimit;
	const uchar* datastart;
	int dims;    // 维度
	int flags;
	int rows;
	MatSize size;
	MatStep step;
	UMatData* u;
```

```
Mat基本上是一个包含两个数据部分的类：矩阵头和指向包含像素值的矩阵的指针。
矩阵头的大小是恒定的，但是矩阵本身的大小可能因图像而异。
OpenCV使用引用计数系统。其思想是每个Mat对象都有自己的标头，但是可以通过让矩阵指针指向同一地址来在两个Mat对象之间共享矩阵。赋值运算符和复制构造函数仅复制标头。

可以创建仅引用完整数据的一部分的标头。例如，要在图像中创建感兴趣的区域 (ROI)，只需创建一个具有新边界的标头：
Mat D (A, Rect(10, 10, 100, 100) ); // using a rectangle
Mat E = A(Range::all(), Range(1,3)); // using row and column boundaries

有时您也想复制矩阵本身，因此OpenCV提供了cv::Mat::clone()和cv::Mat::copyTo()函数。
Mat F = A.clone();
Mat G;
A.copyTo(G);

颜色系统：
RGB 是最常见的，因为我们的眼睛使用类似的东西，但请记住，OpenCV标准显示系统使用BGR颜色空间（红色和蓝色通道交换位置）组成颜色。
HSV和HLS将颜色分解为色调、饱和度和明度/亮度分量，这是我们描述颜色更自然的方式。例如，您可能会忽略最后一个组件，从而使您的算法对输入图像的光线条件不太敏感。
流行的JPEG图像格式使用YCrCb。
CIE L*a*b*是一种感知均匀的色彩空间，如果您需要测量给定颜色与另一种颜色的距离，它会非常方便。

可以使用Mat的<<运算符输出图像的数值矩阵。请注意，这仅适用于二维矩阵。

您可以通过多种方式创建 Mat 对象：
	Mat M(2,2, CV_8UC3, Scalar(0,0,255));
	cout << "M = " << endl << " " << M << endl << endl;
对于二维和多通道图像，我们首先定义它们的大小：按行数和列数。
然后我们需要指定用于存储元素的数据类型和每个矩阵点的通道数。为了实现这一点，我们根据以下约定构建了多个定义：
CV_[The number of bits per item][Signed or Unsigned][Type Prefix]C[The channel number]
例如，CV_8UC3表示我们使用8位长的无符号字符类型，每个像素有三个这样的类型来形成三个通道。最多有四个通道的预定义类型。
cv::Scalar是第四个参数，一个短向量。可以使用该值初始化所有矩阵点。
MATLAB样式初始化：cv::Mat::zeros，cv::Mat::ones，cv::Mat::eye。指定要使用的大小和数据类型：
    Mat E = Mat::eye(4, 4, CV_64F);
    cout << "E = " << endl << " " << E << endl << endl;
    Mat O = Mat::ones(2, 2, CV_32F);
    cout << "O = " << endl << " " << O << endl << endl;
    Mat Z = Mat::zeros(3,3, CV_8UC1);
    cout << "Z = " << endl << " " << Z << endl << endl;
对于小矩阵，您可以使用逗号分隔的初始化器或初始化器列表（最后一种情况下需要 C++11 支持）：
    Mat C = (Mat_<double>(3,3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
    cout << "C = " << endl << " " << C << endl << endl;
    C = (Mat_<double>({0, -1, 0, -1, 5, -1, 0, -1, 0})).reshape(3);
    cout << "C = " << endl << " " << C << endl << endl;
为现有的Mat对象创建一个新标头并对其进行cv::Mat::clone或cv::Mat::copyTo。
    Mat RowClone = C.row(1).clone();
    cout << "RowClone = " << endl << " " << RowClone << endl << endl;
您可以使用 cv::randu() 函数用随机值填充矩阵。您需要为随机值指定下限和上限：
    Mat R = Mat(3, 2, CV_8UC3);
    randu(R, Scalar::all(0), Scalar::all(255));

OpenCV 允许您格式化矩阵输出：
默认：
    cout << "R (default) = " << endl <<        R           << endl << endl;
Python：
    cout << "R (python)  = " << endl << format(R, Formatter::FMT_PYTHON) << endl << endl;
Comma separated values (CSV)：
    cout << "R (csv)     = " << endl << format(R, Formatter::FMT_CSV   ) << endl << endl;
Numpy：
    cout << "R (numpy)   = " << endl << format(R, Formatter::FMT_NUMPY ) << endl << endl;
C：
    cout << "R (c)       = " << endl << format(R, Formatter::FMT_C     ) << endl << endl;

OpenCV也通过<<运算符支持其他常见OpenCV数据结构的输出：
2D Point：
    Point2f P(5, 1);
    cout << "Point (2D) = " << P << endl << endl;
3D Point：
    Point3f P3f(2, 6, 7);
    cout << "Point (3D) = " << P3f << endl << endl;
std::vector via cv::Mat：
    vector<float> v;
    v.push_back( (float)CV_PI);   v.push_back(2);    v.push_back(3.01f);
    cout << "Vector of floats via Mat = " << Mat(v) << endl << endl;
std::vector of points：
    vector<Point2f> vPoints(20);
    for (size_t i = 0; i < vPoints.size(); ++i)
        vPoints[i] = Point2f((float)(i * 5), (float)(i % 7));
    cout << "A vector of 2D Points = " << vPoints << endl << endl;
```
## 如何使用OpenCV扫描图像、查找表和时间测量
```
对于RGB图像，可以形成1600万种颜色。处理如此多的颜色可能会严重影响我们的算法性能。然而，有时只需处理一小部分就足以获得相同的结果。
在这种情况下，我们通常会进行色彩空间缩减（color space reduction）。意味着我们将色彩空间当前值除以一个输入值，最终得到更少的颜色。例如，0到9之间的每个值都取新值0，10到19之间的每个值都为10，依此类推。

对于较大的图像，最好使用查找表，查找表是简单的数组（具有一个或多个维度），其中每个元素对应于输入像素值的一个映射值。它的优势在于我们不需要进行计算，只需读取结果即可。
查找表是一个长度为256的一维数组（针对8位图像，即像素值范围[0, 255]）。
索引i对应输入像素值，查找表中的值lut[i]是该像素值的映射结果。
例如，若lut[100] = 200，则输入图像中所有值为100的像素都会被替换为200。
若需对不同的颜色通道应用不同的查找表，需手动拆分通道后单独处理。

OpenCV提供了两个简单的函数来测量时间cv::getTickCount()和cv::getTickFrequency()。第一个返回从某个事件（比如自启动系统以来）开始的系统CPU的tick数。第二个返回CPU一秒钟tick的次数。因此，测量两个操作之间经过的时间非常简单：
double t = (double)getTickCount();
// do something ...
t = ((double)getTickCount() - t)/getTickFrequency();
cout << "Times passed in seconds: " << t << endl;
```
图像矩阵在内存中是如何存储的？
对于灰度图像，我们有如下内容：
![](images/opencv_1.png)
对于多通道图像，列包含的子列数量与通道数量相同。例如，在BGR颜色系统的情况下：
![](images/opencv_2.png)
请注意，通道的顺序是相反的：BGR而不是RGB。因为在许多情况下，内存足够大，可以以连续的方式存储行，所以行可能一个接一个地排列，创建单个长行。因为所有内容都位于一个位置，一个接一个，这可能有助于加快扫描过程。我们可以使用 cv::Mat::isContinuous() 函数来询问矩阵是否是这种情况。

就性能而言，经典的 C 风格运算符[]（指针）访问是无可比拟的。因此，我们推荐的最有效的赋值方法是：
```cpp
Mat& ScanImageAndReduceC(Mat& I, const uchar* const table)
{
    // accept only char type matrices
    CV_Assert(I.depth() == CV_8U);

    int channels = I.channels();

    int nRows = I.rows;
    int nCols = I.cols * channels;

    if (I.isContinuous())
    {
        nCols *= nRows;
        nRows = 1;
    }

    int i,j;
    uchar* p;
    for( i = 0; i < nRows; ++i)
    {
        p = I.ptr<uchar>(i);
        for ( j = 0; j < nCols; ++j)
        {
            p[j] = table[p[j]];
        }
    }
    return I;
}
```
或者
```cpp
uchar* p = I.data;

for( unsigned int i = 0; i < ncol*nrows; ++i)
    *p++ = table[*p];
```
使用迭代器遍历（对于彩色图像，每列有三个uchar项。这可以视为uchar项的短向量，在OpenCV中以Vec3b命名）：
```cpp
Mat& ScanImageAndReduceIterator(Mat& I, const uchar* const table)
{
    // accept only char type matrices
    CV_Assert(I.depth() == CV_8U);

    const int channels = I.channels();
    switch(channels)
    {
    case 1:
        {
            MatIterator_<uchar> it, end;
            for( it = I.begin<uchar>(), end = I.end<uchar>(); it != end; ++it)
                *it = table[*it];
            break;
        }
    case 3:
        {
            MatIterator_<Vec3b> it, end;
            for( it = I.begin<Vec3b>(), end = I.end<Vec3b>(); it != end; ++it)
            {
                (*it)[0] = table[(*it)[0]];
                (*it)[1] = table[(*it)[1]];
                (*it)[2] = table[(*it)[2]];
            }
        }
    }

    return I;
}
```
在图像处理中，将给定图像的所有值修改为其他值是很常见的。OpenCV提供了修改图像值的函数，不需要自己写图像的扫描逻辑，我们使用core模块的cv::LUT()函数，首先我们建立一个Mat类型的查找表：
```cpp
    Mat lookUpTable(1, 256, CV_8U);
    uchar* p = lookUpTable.ptr();
    for( int i = 0; i < 256; ++i)
        p[i] = table[i];
```
最后调用函数（I是我们的输入图像，J是输出图像）：
```cpp
        LUT(I, lookUpTable, J);
```
如果可能的话，使用OpenCV的现有函数（而不是重新发明这些函数）。最快的方法是LUT函数。这是因为OpenCV库通过英特尔线程构建块启用了多线程。但是，如果您需要编写一个简单的图像扫描，最好使用指针方法。迭代器是更安全的选择，但速度较慢。
## 矩阵的掩码运算
```
矩阵上的掩码操作非常简单。即根据掩码矩阵重新计算图像中每个像素的值。此掩码包含的值将调整相邻像素（和当前像素）对新像素值的影响程度。从数学角度来看，我们使用指定的值进行加权平均值。
```
在图像处理中，应用此类过滤器非常常见，因此OpenCV中有一个函数负责应用掩码（在某些地方也称为内核）。为此，您首先需要定义一个保存掩码的对象：
```cpp
    Mat kernel = (Mat_<char>(3,3) <<  0, -1,  0,
                                   -1,  5, -1,
                                    0, -1,  0);
```
然后调用filter2D()函数指定要使用的输入、输出图像和内核：
```cpp
    filter2D( src, dst1, src.depth(), kernel );
```
该函数甚至有第五个可选参数来指定内核的中心，第六个参数用于在将过滤后的像素存储到K中之前为其添加一个可选值，第七个参数用于确定在操作未定义的区域（边界）中要执行的操作。
## 图像的基本操作
```cpp
// 从文件加载图像：
        Mat img = imread(filename);
// 如果读取 jpg 文件，则默认会创建 3 通道图像。如果需要灰度图像，请使用：
        Mat img = imread(filename, IMREAD_GRAYSCALE);
// 输入文件的格式由其内容（前几个字节）决定。
// 要将图像保存到文件：
        imwrite(filename, img);
// 输出文件的格式由其扩展名决定。
// 使用 cv::imdecode 和 cv::imencode 从内存而不是文件读取和写入图像。

// 为了获取像素强度值，您必须知道图像的类型和通道数。以下是单通道灰度图像（类型 8UC1）和像素坐标 x 和 y 的示例：
            Scalar intensity = img.at<uchar>(y, x);
// 或者，您可以使用以下符号（仅限 C++）：
            Scalar intensity = img.at<uchar>(Point(x, y));
// 现在让我们考虑具有 BGR 颜色排序的 3 通道图像（imread 返回的默认格式）：
            Vec3b intensity = img.at<Vec3b>(y, x);
            uchar blue = intensity.val[0];
            uchar green = intensity.val[1];
            uchar red = intensity.val[2];
// 您可以对浮点图像使用相同的方法（例如，您可以通过在 3 通道图像上运行 Sobel 来获得这样的图像）（仅限 C++）：
            Vec3f intensity = img.at<Vec3f>(y, x);
            float blue = intensity.val[0];
            float green = intensity.val[1];
            float red = intensity.val[2];
// 可以使用相同的方法来改变像素强度：
            img.at<uchar>(y, x) = 128;
// 可以为每个函数提供一个空的输出 Mat。每个实现都调用 Mat::create 来获取目标矩阵。如果矩阵为空，则此方法会为矩阵分配数据。如果矩阵不为空且大小和类型正确，则此方法不执行任何操作。但是，如果大小或类型与输入参数不同，则数据将被释放（并丢失），并分配新数据。例如：
        Mat img = imread("image.jpg");
        Mat sobelx;
        Sobel(img, sobelx, CV_32F, 1, 0);


// 矩阵上定义了许多方便的运算符。例如，下面介绍如何从现有的灰度图像 img 制作黑色图像
            img = Scalar(0);
// 选择感兴趣的区域：
            Rect r(10, 10, 100, 100);
            Mat smallImg = img(r);
// 从彩色到灰度的转换：
        Mat img = imread("image.jpg"); // loading a 8UC3 image
        Mat grey;
        cvtColor(img, grey, COLOR_BGR2GRAY);
// 将图像类型从 8UC1 更改为 32FC1：
        src.convertTo(dst, CV_32F);
```

```cpp
// 在开发过程中查看算法的中间结果非常有用。OpenCV 提供了一种可视化图像的便捷方式。可以使用以下方式显示 8U 图像：
        Mat img = imread("image.jpg");
        namedWindow("image", WINDOW_AUTOSIZE);
        imshow("image", img);
        waitKey();
// 调用 waitKey() 会启动一个消息传递循环，等待“图像”窗口中的按键。32F 图像需要转换为 8U 类型。例如：
        Mat img = imread("image.jpg");
        Mat grey;
        cvtColor(img, grey, COLOR_BGR2GRAY);
        Mat sobelx;
        Sobel(grey, sobelx, CV_32F, 1, 0);
        double minVal, maxVal;
        minMaxLoc(sobelx, &minVal, &maxVal); //find minimum and maximum intensities
        Mat draw;
        sobelx.convertTo(draw, CV_8U, 255.0/(maxVal - minVal), -minVal * 255.0/(maxVal - minVal));
        namedWindow("image", WINDOW_AUTOSIZE);
        imshow("image", draw);
        waitKey();
// 这里 cv::namedWindow 不是必需的，因为它后面紧跟着 cv::imshow。不过，它可以用于更改窗口属性，或者在使用 cv::createTrackbar 时使用
```
## 使用 OpenCV 添加（混合）两幅图像
```cpp
// 线性混合（linear blending）：g(x) = (1 - 𝛼)A(x) + 𝛼B(x)
// 通过将 𝛼 从 0 →​​1 改变，此运算符可用于在两个图像或视频之间执行时间交叉溶解，如幻灯片和电影制作中所见（很酷，是吧？）

//src1 和 src2，它们必须具有相同的尺寸（宽度和高度）和类型。
#include "opencv2/imgcodecs.hpp"
#include "opencv2/highgui.hpp"
#include <iostream>

using namespace cv;

// we're NOT "using namespace std;" here, to avoid collisions between the beta variable and std::beta in c++17
using std::cin;
using std::cout;
using std::endl;

int main( void )
{
   double alpha = 0.5; double beta; double input;

   Mat src1, src2, dst;

   cout << " Simple Linear Blender " << endl;
   cout << "-----------------------" << endl;
   cout << "* Enter alpha [0.0-1.0]: ";
   cin >> input;

   // We use the alpha provided by the user if it is between 0 and 1
   if( input >= 0 && input <= 1 )
     { alpha = input; }

   src1 = imread( samples::findFile("D:/other/m.jpg") );
   src2 = imread( samples::findFile("D:/other/n.jpg") );

   if( src1.empty() ) { cout << "Error loading src1" << endl; return EXIT_FAILURE; }
   if( src2.empty() ) { cout << "Error loading src2" << endl; return EXIT_FAILURE; }

   beta = ( 1.0 - alpha );
   addWeighted( src1, alpha, src2, beta, 0.0, dst);

   imshow( "Linear Blend", dst );
   waitKey(0);

   return 0;
}
```
## 改变图像的对比度和亮度
```
通用图像处理算子是一种获取一个或多个输入图像并生成输出图像的函数。
图像变换可以看作：
	Point operators (pixel transforms)
	Neighborhood (area-based) operators

Pixel Transforms示例包括亮度和对比度调整以及颜色校正和转换。
亮度和对比度调整：g(x) = 𝛼f(x) + 𝛽
参数 𝛼 > 0 和 𝛽 通常被称为gain和bias参数；有时这些参数分别控制对比度和亮度。
```

```cpp

#include "opencv2/imgcodecs.hpp"
#include "opencv2/highgui.hpp"
#include <iostream>

// we're NOT "using namespace std;" here, to avoid collisions between the beta variable and std::beta in c++17
using std::cin;
using std::cout;
using std::endl;
using namespace cv;

int main( int argc, char** argv )
{
    CommandLineParser parser( argc, argv, "{@input | lena.jpg | input image}" );
    Mat image = imread( samples::findFile( parser.get<String>( "@input" ) ) );
    if( image.empty() )
    {
      cout << "Could not open or find the image!\n" << endl;
      cout << "Usage: " << argv[0] << " <Input image>" << endl;
      return -1;
    }

    Mat new_image = Mat::zeros( image.size(), image.type() );

    double alpha = 1.0; /*< Simple contrast control */
    int beta = 0;       /*< Simple brightness control */

    cout << " Basic Linear Transforms " << endl;
    cout << "-------------------------" << endl;
    cout << "* Enter the alpha value [1.0-3.0]: "; cin >> alpha;
    cout << "* Enter the beta value [0-100]: ";    cin >> beta;

    for( int y = 0; y < image.rows; y++ ) {
        for( int x = 0; x < image.cols; x++ ) {
            for( int c = 0; c < image.channels(); c++ ) {
                new_image.at<Vec3b>(y,x)[c] =
                  saturate_cast<uchar>( alpha*image.at<Vec3b>(y,x)[c] + beta );
            }
        }
    }

    imshow("Original Image", image);
    imshow("New Image", new_image);

    waitKey();
    return 0;
}
```

```cpp
// 我们不需要使用 for 循环来访问每个像素，而是可以直接使用以下命令：
image.convertTo(new_image, -1, alpha, beta);
// 两种方法都会产生相同的结果，但convertTo更优化并且运行速度更快。
```

```
增加（/减少） 𝛽 值将会为每个像素添加（/减去）一个常数值。[0,255]范围之外的像素值将会饱和（即，高于（/小于）255（/ 0）的像素值将被限制为 255（/ 0））。
暗图像将具有许多颜色值较低的像素。
可能会发生这种情况：使用 𝛽 偏差会提高亮度，但同时图像会出现slight veil，因为对比度会降低。𝛼 增益可用于减弱这种影响，但由于饱和度，我们将丢失原始明亮区域的一些细节。
```
伽马校正可用于通过在输入值和映射的输出值之间使用非线性变换来校正图像的亮度：
![](images/opencv_4.png)
由于此关系是非线性的，因此对于所有像素而言，效果不会相同，并且取决于其原始值。
![](images/opencv_5.png)
当 𝛾 < 1 时，原始暗区将变得更亮
```cpp
// 伽马校正
    Mat lookUpTable(1, 256, CV_8U);
    uchar* p = lookUpTable.ptr();
    for( int i = 0; i < 256; ++i)
        p[i] = saturate_cast<uchar>(pow(i / 255.0, gamma_) * 255.0);

    Mat res = img.clone();
    LUT(img, lookUpTable, res);
```
## 离散傅里叶变换
以下是 dft() 的用法示例：
```cpp
#include "opencv2/core.hpp"
#include "opencv2/imgproc.hpp"
#include "opencv2/imgcodecs.hpp"
#include "opencv2/highgui.hpp"

#include <iostream>

using namespace cv;
using namespace std;

static void help(char ** argv)
{
    cout << endl
        <<  "This program demonstrated the use of the discrete Fourier transform (DFT). " << endl
        <<  "The dft of an image is taken and it's power spectrum is displayed."  << endl << endl
        <<  "Usage:"                                                                      << endl
        << argv[0] << " [image_name -- default lena.jpg]" << endl << endl;
}

int main(int argc, char ** argv)
{
    help(argv);

    const char* filename = argc >=2 ? argv[1] : "lena.jpg";

    Mat I = imread( samples::findFile( filename ), IMREAD_GRAYSCALE);
    if( I.empty()){
        cout << "Error opening image" << endl;
        return EXIT_FAILURE;
    }

    Mat padded;                            //expand input image to optimal size
    int m = getOptimalDFTSize( I.rows );
    int n = getOptimalDFTSize( I.cols ); // on the border add zero values
    copyMakeBorder(I, padded, 0, m - I.rows, 0, n - I.cols, BORDER_CONSTANT, Scalar::all(0));

    Mat planes[] = {Mat_<float>(padded), Mat::zeros(padded.size(), CV_32F)};
    Mat complexI;
    merge(planes, 2, complexI);         // Add to the expanded another plane with zeros

    dft(complexI, complexI);            // this way the result may fit in the source matrix

    // compute the magnitude and switch to logarithmic scale
    // => log(1 + sqrt(Re(DFT(I))^2 + Im(DFT(I))^2))
    split(complexI, planes);                   // planes[0] = Re(DFT(I), planes[1] = Im(DFT(I))
    magnitude(planes[0], planes[1], planes[0]);// planes[0] = magnitude
    Mat magI = planes[0];

    magI += Scalar::all(1);                    // switch to logarithmic scale
    log(magI, magI);

    // crop the spectrum, if it has an odd number of rows or columns
    magI = magI(Rect(0, 0, magI.cols & -2, magI.rows & -2));

    // rearrange the quadrants of Fourier image  so that the origin is at the image center
    int cx = magI.cols/2;
    int cy = magI.rows/2;

    Mat q0(magI, Rect(0, 0, cx, cy));   // Top-Left - Create a ROI per quadrant
    Mat q1(magI, Rect(cx, 0, cx, cy));  // Top-Right
    Mat q2(magI, Rect(0, cy, cx, cy));  // Bottom-Left
    Mat q3(magI, Rect(cx, cy, cx, cy)); // Bottom-Right

    Mat tmp;                           // swap quadrants (Top-Left with Bottom-Right)
    q0.copyTo(tmp);
    q3.copyTo(q0);
    tmp.copyTo(q3);

    q1.copyTo(tmp);                    // swap quadrant (Top-Right with Bottom-Left)
    q2.copyTo(q1);
    tmp.copyTo(q2);

    normalize(magI, magI, 0, 1, NORM_MINMAX); // Transform the matrix with float values into a
                                            // viewable image form (float between values 0 and 1).

    imshow("Input Image"       , I   );    // Show the result
    imshow("spectrum magnitude", magI);
    waitKey();

    return EXIT_SUCCESS;
}
```

```
傅里叶变换将把图像分解为正弦和余弦分量。换句话说，它会将图像从空间域转换为频域。这个想法是，任何函数都可以用无限正弦函数和余弦函数的总和来精确近似。傅里叶变换是实现此目的的一种方法。从数学上讲，二维图像的傅里叶变换为：
```
![](images/opencv_6.png)
```
其中 f 是图像在空间域中的值，F 是图像在频率域中的值。变换的结果是复数。可以通过a real image and a complex image或通过a magnitude and a phase image来显示。
然而，在整个图像处理算法中，只有幅度图像是有趣的，因为它包含了我们需要的关于图像几何结构的所有信息。不过，如果您打算对这些形式的图像进行一些修改，然后需要重新转换它，那么您需要保留这两者。

在此示例中，我将展示如何计算和显示傅里叶变换的幅度图像。假设数字图像是离散的。这意味着它们可能从给定的域值中获取一个值。例如，在基本灰度图像中，值通常介于 0 和 255 之间。因此，傅里叶变换也需要是离散类型，从而产生离散傅里叶变换 (DFT)。每当您需要从几何角度确定图像的结构时，您都需要使用它。以下是要遵循的步骤（以灰度输入图像 I 为例）：
将图像扩展至最佳尺寸：
DFT 的性能取决于图像大小。对于二、三和五的倍数的图像，它往往是最快的。因此，为了实现最佳性能，通常最好填充图像的边框值以获得具有此类特征的尺寸。getOptimalDFTSize() 返回此最佳尺寸，我们可以使用 copyMakeBorder() 函数来扩展图像的边框（附加的像素初始化为零）：
    Mat padded;                            //expand input image to optimal size
    int m = getOptimalDFTSize( I.rows );
    int n = getOptimalDFTSize( I.cols ); // on the border add zero values
    copyMakeBorder(I, padded, 0, m - I.rows, 0, n - I.cols, BORDER_CONSTANT, Scalar::all(0));
Make place for both the complex and the real values：
傅里叶变换的结果很复杂。这意味着对于每个图像值，结果都是两个图像值（每个组件一个）。此外，频域范围比其空间对应范围大得多。因此，我们通常至少以浮点格式存储这些。因此，我们将输入图像转换为这种类型，并使用另一个通道对其进行扩展以保存复数值：
    Mat planes[] = {Mat_<float>(padded), Mat::zeros(padded.size(), CV_32F)};
    Mat complexI;
    merge(planes, 2, complexI);         // Add to the expanded another plane with zeros
进行离散傅里叶变换：
可以进行就地计算（输入与输出相同）：
    dft(complexI, complexI);            // this way the result may fit in the source matrix
Transform the real and complex values to magnitude：
复数有实部 (Re) 和复数 (虚部 - Im) 部分。DFT 的结果为复数。DFT 的幅度为：
```
![](images/opencv_7.png)
```
翻译成OpenCV代码：
    split(complexI, planes);                   // planes[0] = Re(DFT(I), planes[1] = Im(DFT(I))
    magnitude(planes[0], planes[1], planes[0]);// planes[0] = magnitude
    Mat magI = planes[0];
切换到对数刻度：
事实证明，傅里叶系数的动态范围太大，无法在屏幕上显示。我们有一些小的和一些高的变化值，我们无法这样观察。因此，高值将全部显示为白点，而小值将显示为黑点。要使用灰度值进行可视化，我们可以将线性刻度转换为对数刻度：M1 = log(1 + M)
翻译成OpenCV代码：
    magI += Scalar::all(1);                    // switch to logarithmic scale
    log(magI, magI);
裁剪并重新排列：
还记得我们在第一步扩大了图像吗？现在是时候丢弃新引入的值了。为了可视化的目的，我们还可以重新排列结果的象限，以便原点（零，零）与图像中心相对应。
    // crop the spectrum, if it has an odd number of rows or columns
    magI = magI(Rect(0, 0, magI.cols & -2, magI.rows & -2));

    // rearrange the quadrants of Fourier image  so that the origin is at the image center
    int cx = magI.cols/2;
    int cy = magI.rows/2;

    Mat q0(magI, Rect(0, 0, cx, cy));   // Top-Left - Create a ROI per quadrant
    Mat q1(magI, Rect(cx, 0, cx, cy));  // Top-Right
    Mat q2(magI, Rect(0, cy, cx, cy));  // Bottom-Left
    Mat q3(magI, Rect(cx, cy, cx, cy)); // Bottom-Right

    Mat tmp;                           // swap quadrants (Top-Left with Bottom-Right)
    q0.copyTo(tmp);
    q3.copyTo(q0);
    tmp.copyTo(q3);

    q1.copyTo(tmp);                    // swap quadrant (Top-Right with Bottom-Left)
    q2.copyTo(q1);
    tmp.copyTo(q2);
Normalize：
再次执行此操作是为了实现可视化。我们现在有了幅度，但这仍然超出了 0 到 1 的图像显示范围。我们使用 cv::normalize() 函数将值标准化到此范围。
    normalize(magI, magI, 0, 1, NORM_MINMAX); // Transform the matrix with float values into a
                                            // viewable image form (float between values 0 and 1).
```
## 使用 XML/YAML/JSON 文件进行文件输入和输出
```cpp
// XML/YAML/JSON 文件打开和关闭（FileStorage::WRITE、FileStorage::READ、FileStorage::APPEND）
FileStorage fs(filename, FileStorage::WRITE);
FileStorage fs;
fs.open(filename, FileStorage::WRITE);
// 文件名中指定的扩展名还决定了将使用的输出格式。
// 当 cv::FileStorage 对象被销毁时，文件会自动关闭。但是，你可以使用 release 函数明确调用此方法：
fs.release();                                       // explicit close

// 文本和数字的输入和输出。
fs << "iterationNr" << 100;
int itNr;
//fs["iterationNr"] >> itNr;
itNr = (int) fs["iterationNr"];
```
## 如何使用OpenCV的parallel_for_来并行化你的代码
```
第一个先决条件是使用并行框架构建 OpenCV。在 OpenCV 4.5 中，按以下顺序提供并行框架：
Intel 线程构建模块（第三方库，应明确启用）
OpenMP（集成到编译器，应明确启用）
Windows RT 并发性（系统范围，自动使用（仅限 Windows RT））
Windows 并发（运行时的一部分，自动使用（仅限 Windows - MSVC++ >= 10））
Pthreads
```