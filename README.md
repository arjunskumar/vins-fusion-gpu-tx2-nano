# vins-fusion-gpu-tx2
Installation step of vins-fusion gpu version on Nvidia Jetson TX2 ( JP 4.2.2)
# Prerequisites
## Ceres solver and Eigen : Mandatory for VINS
### Eigen home
```
$  sudo apt-get remove libeigen3-dev #  Remove preb-uilt Eigen
$ cd ~/Downloads/
$ wget -O eigen.zip http://bitbucket.org/eigen/eigen/get/3.3.7.zip #check version
$ unzip eigen.zip -d eigen-3.3.7
$ mkdir eigen-build && cd eigen-build
$ cmake ../eigen-3.3.7/ && sudo make install
$ pkg-config --modversion eigen3 # Check Eigen Version
```
![Drag Racing](./img/md1.png)

### Ceres solver home

```
$ sudo apt-get install -y cmake libgoogle-glog-dev libatlas-base-dev libsuitesparse-dev
$ wget http://ceres-solver.org/ceres-solver-1.14.0.tar.gz
$ tar zxf ceres-solver-1.14.0.tar.gz
$ mkdir ceres-bin
$ mkdir solver && cd ceres-bin
$ cmake ../ceres-solver-1.14.0 -DEXPORT_BUILD_DIR=ON -DCMAKE_INSTALL_PREFIX="../solver"  #good for build without being root privileged and at wanted directory
$ make -j3 # 8 : number of cores
$ make test
$ make install
```
