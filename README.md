# DTAM Dense Tracking and Mapping  直接法 稠密跟踪和建图

![](https://img-blog.csdn.net/20160406220311974?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

      目前流行的大多数VSLAM都是基于鲁棒的特征检测和跟踪，
      DTAM是基于单像素的方法，
      采用了在稠密地图（densemap）创建和相机位置估计都很有优势的低基线帧（lowbaseline frame）。
      像PATM一样，它分为两个部分：姿势跟踪和3d 映射。
      PTAM仅仅跟踪稀疏的3d点，
      DTAM保持了对关键帧的稠密深度映射（densedepth map）。
      
      lsd_slam也是直接法，不过是稀疏的地图，跟踪灰度梯度大的像素点(单目相机)
      
      DTAM继承了关键帧的架构，但对关键帧的处理和传统的特征点提取有很大的不同。
      相比传统方法对每一帧进行很稀疏的特征点提取，
      DTAM的direct method在默认环境亮度不变（brightness consistancy assumption）的前提下，
      对每一个像素的深度数据进行inverse depth的提取和不断优化来建立稠密地图并实现稳定的位置跟踪。

      用比较直观的数字对比来说明DTAM和PTAM的区别：
          DTAM的一个关键帧有30万个像素的深度估计，而PTAM一般的做法是最多一千个。

      DTAM的优缺点很明显（具体对比见图1）：
          准、稳，但速度是问题，每一个像素都计算，确实容易通过GPU并行计算，但功耗和产品化的难度也都随之升高。

OpenDTAM
========

An open source implementation of DTAM
On Ubuntu, you need the qtbase5-dev and libopencv-dev packages
This project is no longer currently active, but I will try to provide suggestions as possible. I would love to get back to it, but my life is currently quite busy. 

## Build Instructions On Ubuntu 12.04 LTS

For building `OpenDTAM`, here is a brief instruction on Ubuntu 12.04 LTS.

### Install dependencies

#### qtbase5-dev

    sudo apt-add-repository ppa:ubuntu-sdk-team/ppa
    sudo apt-get update
    sudo apt-get install qtbase5-dev

#### libopencv-dev

    sudo apt-get install libopencv-dev

#### boost

    sudo apt-get install libboost1.48-all-dev

### Build OpenDTAM

    cd OpenDTAM/Cpp
    mkdir Build
    cd Build
    cmake ..
    make

### Run OpenDTAM

    ./a.out

### Trouble Shooting

Running  "pkg-config --modversion opencv" will tell you what version you have. Hopefully it is 
close to 2.4.9 which is what commit a5f91d0cd58c3353ef56a3694648375f1c053082  was built for.

You may have problems with the versions of the dependencies, if so you may be able to resolve them by installing the required ones according to the messages output by `cmake`.

The `Trajectory_30_seconds` directory may reside in different path in your system, you can modify them in `testprog.cpp` before running `make`.

Or if none of this works compile a version of OpenCV from source on your machine.  
Then send me an email with the output of cmake (something like " -- Detected version of GNU GCC: 46 (406) .......")  
and also the output of   
"cmake -L"  
and I can tell you what you need to do.
