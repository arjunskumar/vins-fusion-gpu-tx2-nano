# vins-fusion-gpu-tx2
Installation step of vins-fusion gpu version on Nvidia Jetson TX2 ( JP 4.2.2)
# Prerequisites

### ROS-Melodic
```
$ cd ~/Downloads/
$ git clone https://github.com/roboticsengineer93/vins-fusion-gpu-tx2.git
$ cd vins-fusion-gpu-tx2/
$ installROS.sh -p  ros-melodic-desktop-full
$ setupCatkinWorkspace.sh
```
### Eigen 
```
$ sudo apt-get remove libeigen3-dev #  Remove preb-uilt Eigen
$ cd ~/Downloads/
$ wget -O eigen.zip http://bitbucket.org/eigen/eigen/get/3.3.7.zip #check version
$ unzip eigen.zip -d eigen-3.3.7
$ mkdir eigen-build && cd eigen-build
$ cmake ../eigen-3.3.7/ && sudo make install
$ pkg-config --modversion eigen3 # Check Eigen Version
```
![Eigen-img](./img/md1.png)

### Ceres solver 

```
$ cd ~/Downloads/
$ sudo apt-get install -y cmake libgoogle-glog-dev libatlas-base-dev libsuitesparse-dev
$ wget http://ceres-solver.org/ceres-solver-1.14.0.tar.gz
$ tar zxf ceres-solver-1.14.0.tar.gz
$ mkdir ceres-bin
$ mkdir solver && cd ceres-bin
$ cmake ../ceres-solver-1.14.0 -DEXPORT_BUILD_DIR=ON -DCMAKE_INSTALL_PREFIX="../solver" 
  #good for build without being root privileged and at wanted directory
$ make -j3 # 6 : number of cores
$ make test
$ make install
$ bin/simple_bundle_adjuster ../ceres-solver-1.14.0/data/problem-16-22106-pre.txt # to check version
```
![ceres-solver-img](./img/md2.png)

## Opencv

```
$ sudo apt-get purge libopencv* python-opencv # remove prebuilt opencv
$ sudo apt-get update
$ sudo apt-get install -y build-essential pkg-config
$ sudo apt-get install -y cmake libavcodec-dev libavformat-dev libavutil-dev \
    libglew-dev libgtk2.0-dev libgtk-3-dev libjpeg-dev libpng-dev libpostproc-dev \
    libswscale-dev libtbb-dev libtiff5-dev libv4l-dev libxvidcore-dev \
    libx264-dev qt5-default zlib1g-dev libgl1 libglvnd-dev pkg-config \
    libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev mesa-utils 
    ## libeigen3-dev # recommend to build from source : http://eigen.tuxfamily.org/index.php?title=Main_Page
$ sudo apt-get install python2.7-dev python3-dev python-numpy python3-numpy
```

```
# To fix OpenGL related compilation problems 

$ cd /usr/lib/aarch64-linux-gnu/
$ sudo ln -sf libGL.so.1.0.0 libGL.so
$ sudo vim /usr/local/cuda/include/cuda_gl_interop.h

# Comment (line #62~68) of cuda_gl_interop.h 

//#if defined(__arm__) || defined(__aarch64__)
//#ifndef GL_VERSION
//#error Please include the appropriate gl headers before including cuda_gl_interop.h
//#endif
//#else
 #include <GL/gl.h>
//#endif
```

```
# Then once linking is done, go to Downloads ro begin opencv installation
$ cd ~/Downloads/
$ wget -O opencv.zip https://github.com/opencv/opencv/archive/3.4.1.zip # check version
$ unzip opencv.zip
$ cd opencv-3.4.1/ && mkdir build && cd build
$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D WITH_CUDA=ON \
        -D CUDA_ARCH_BIN=6.2 \
        -D CUDA_ARCH_PTX="" \
        -D ENABLE_FAST_MATH=ON \
        -D CUDA_FAST_MATH=ON \
        -D WITH_CUBLAS=ON \
        -D WITH_LIBV4L=ON \
        -D WITH_GSTREAMER=ON \
        -D WITH_GSTREAMER_0_10=OFF \
        -D WITH_QT=ON \
        -D WITH_OPENGL=ON \
        -D CUDA_NVCC_FLAGS="--expt-relaxed-constexpr" \
        -D WITH_TBB=ON \
         ../
$ make  # running in single core is good to resolve the compilation issues         
$ sudo make install
```
