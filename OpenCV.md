截至2025年1月，opencv的最新版本为4.11.0，opencv的4.0.0版本发布于2018年11月18日。
opencv中所有类和函数都放在cv命名空间里。
opencv会处理所有底层内存问题。
异常都是cv::Exception的实例或其子类的实例，而cv::Exception是std::exception的子类，所以可以使用c++中的库处理。
opencv的实现是完全可重入的。
## 主要模块
```html
core                Core functionality
imgproc             Image Processing
imgcodecs           Image file reading and writing
videoio             Video I/O
highgui             High-level GUI
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
    return 0;
}
```
