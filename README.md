# vins-fusion-gpu-tx2
Installation step of vins-fusion gpu version on Nvidia Jetson TX2 ( JP 4.2.2)
# Prerequisites

### ROS- Melodic
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
$ make -j3 # 8 : number of cores
$ make test
$ make install
$ bin/simple_bundle_adjuster ../ceres-solver-1.14.0/data/problem-16-22106-pre.txt # to check version
```
![ceres-solver-img](./img/md2.png)
