截至2025年1月，opencv的最新版本为4.11.0，opencv的4.0.0版本发布于2018年11月18日。
opencv中所有类和函数都放在cv命名空间里。
opencv会处理所有底层内存问题。（官方文档：使用OpenCV的C++接口，您不需要考虑内存管理。）
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
## Mat - 基本图像容器
```
Mat基本上是一个包含两个数据部分的类：矩阵头和指向包含像素值的矩阵的指针。
矩阵头的大小是恒定的，但是矩阵本身的大小可能因图像而异，并且通常会大几个数量级。
OpenCV使用引用计数系统。其思想是每个Mat对象都有自己的标头，但是可以通过让矩阵指针指向同一地址来在两个Mat对象之间共享矩阵。赋值运算符和复制构造函数仅复制标头。

可以创建仅引用完整数据的一部分的标头。例如，要在图像中创建感兴趣的区域 (ROI)，只需创建一个具有新边界的标头：
Mat D (A, Rect(10, 10, 100, 100) ); // using a rectangle
Mat E = A(Range::all(), Range(1,3)); // using row and column boundaries

有时您也想复制矩阵本身，因此OpenCV提供了cv::Mat::clone()和cv::Mat::copyTo()函数。
Mat F = A.clone();
Mat G;
A.copyTo(G);

颜色系统：
RGB 是最常见的，因为我们的眼睛使用类似的东西，但请记住，OpenCV 标准显示系统使用 BGR 颜色空间（红色和蓝色通道交换位置）组成颜色。
HSV 和 HLS 将颜色分解为色调、饱和度和明度/亮度分量，这是我们描述颜色更自然的方式。例如，您可能会忽略最后一个组件，从而使您的算法对输入图像的光线条件不太敏感。
流行的 JPEG 图像格式使用 YCrCb。
CIE L*a*b*是一种感知均匀的色彩空间，如果您需要测量给定颜色与另一种颜色的距离，它会非常方便。

可以使用Mat的<<运算符输出图像的数值矩阵。请注意，这仅适用于二维矩阵。

您可以通过多种方式创建 Mat 对象：
	Mat M(2,2, CV_8UC3, Scalar(0,0,255));
	cout << "M = " << endl << " " << M << endl << endl;
对于二维和多通道图像，我们首先定义它们的大小：按行数和列数。
然后我们需要指定用于存储元素的数据类型和每个矩阵点的通道数。为了实现这一点，我们根据以下约定构建了多个定义：
CV_[The number of bits per item][Signed or Unsigned][Type Prefix]C[The channel number]
例如，CV_8UC3 表示我们使用 8 位长的无符号字符类型，每个像素有三个这样的类型来形成三个通道。最多有四个通道的预定义类型。
cv::Scalar是第四个参数，一个短向量。可以使用该值初始化所有矩阵点。如果您需要更多，您可以使用上面的宏创建类型，并在括号中设置频道号，如下所示。
    int sz[3] = {2,2,2};
    Mat L(3,sz, CV_8UC(1), Scalar::all(0));
上面的例子展示了如何创建一个多维矩阵。指定它的维度，然后传递一个包含每个维度大小的指针，其余保持不变。
    M.create(4,4, CV_8UC(2));
    cout << "M = "<< endl << " "  << M << endl << endl;
您无法使用此构造初始化矩阵值。如果新大小不适合旧大小，它将仅重新分配其矩阵数据内存。
MATLAB 样式初始化程序： cv::Mat::zeros ， cv::Mat::ones ， cv::Mat::eye 。指定要使用的大小和数据类型：
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
为现有的 Mat 对象创建一个新标头并对其进行 cv::Mat::clone 或 cv::Mat::copyTo。
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

OpenCV 也通过 << 运算符支持其他常见 OpenCV 数据结构的输出：
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
## 如何使用 OpenCV 扫描图像、查找表和时间测量
```
色彩空间缩减（color space reduction）。意味着我们将色彩空间当前值除以新的输入值，最终得到更少的颜色。例如，0 到 9 之间的每个值都取新值 0，10到19之间的每个值都为10，依此类推。

对于较大的图像，最好事先计算所有可能的值，然后在分配过程中使用查找表进行分配。查找表是简单的数组（具有一个或多个维度），对于给定的输入值变化，它保存最终的输出值。它的优势在于我们不需要进行计算，只需读取结果即可。

OpenCV 提供了两个简单的函数来测量时间 cv::getTickCount() 和 cv::getTickFrequency()。第一个返回从某个事件（比如自启动系统以来）开始的系统 CPU 的滴答数。第二个返回 CPU 在一秒钟内发出滴答声的次数。因此，测量两个操作之间经过的时间量非常简单：
double t = (double)getTickCount();
// do something ...
t = ((double)getTickCount() - t)/getTickFrequency();
cout << "Times passed in seconds: " << t << endl;
```