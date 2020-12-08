Ubuntu20 ROS noetic 自带的OpenCV版本为4.2.0，与ORB SLAM3的OpenCV版本相比，接口有一些变化。
这里按照对应的Opencv4接口进行修改，可以编译通过，暂未测试数据集。

具体的修改内容：  
1、CMakeLists.txt文件    
find_package(OpenCV 3) -> find_package(OpenCV )  

2、与上面修改的VINS_Mono一样，转换OpenCV函数名  
CV_LOAD_IMAGE_UNCHANGED -> cv::IMREAD_UNCHANGED  
CV_GRAY2BGR -> cv::COLOR_GRAY2BGR    
CV_REDUCE_SUM -> cv::REDUCE_SUM  
CV_RGB2GRAY -> cv::COLOR_RGB2GRAY    
CV_RGBA2GRAY -> cv::COLOR_RGBA2GRAY    
CV_BGRA2GRAY -> cv::COLOR_BGRA2GRAY     

3、修改头文件  
ORBextractor.h中  
#include<opencv/cv.h> -> #include<opencv2/core.hpp>

PnPsolver.h中    
添加 #include<opencv2/core/core_c.h> 、#include<opencv2/core.hpp>，其中core_c.h文件包含了一些C数据结构和算法

4、LoopClosing.h中第49行修改为  
typedef map<KeyFrame*,g2o::Sim3,std::less<KeyFrame*>,
        Eigen::aligned_allocator<std::pair<KeyFrame* const, g2o::Sim3> > > KeyFrameAndPose;  

参考：[https://github.com/enteguo/ORB_SLAM3_opencv4.2_ubuntu20.04](https://github.com/enteguo/ORB_SLAM3_opencv4.2_ubuntu20.04)