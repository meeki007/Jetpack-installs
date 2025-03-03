# OpenCV

#### Update your system
```
$ sudo apt update
```
#### set CUDA location
```
$ sudo sh -c "echo '/usr/local/cuda/lib64' >> /etc/ld.so.conf.d/nvidia-tegra.conf"
$ sudo ldconfig
```

#### Install dependencies
Altinstall of python - if you have installed a version of python that did not come with jetpack you must install numpy and pip VIA the python version you installed. Change version number to fit your needs.

***Skip this next command if your using jetpack's python and have not installed another python version.***
```
$ sudo -H python3.9 -m pip install numpy --upgrade numpy
```
All versions of python can install the rest of the dependencies with apt.
```
$ sudo apt-get install -y build-essential cmake git unzip pkg-config zlib1g-dev && \
sudo apt-get install -y libjpeg-dev libjpeg8-dev libjpeg-turbo8-dev libpng-dev libtiff-dev && \
sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libglew-dev && \
sudo apt-get install -y libgtk2.0-dev libgtk-3-dev libcanberra-gtk* && \
sudo apt-get install -y python-dev python-numpy python-pip && \
sudo apt-get install -y python3-dev python3-numpy python3-pip && \
sudo apt-get install -y libxvidcore-dev libx264-dev libgtk-3-dev && \
sudo apt-get install -y libtbb2 libtbb-dev libdc1394-22-dev libxine2-dev && \
sudo apt-get install -y gstreamer1.0-tools libv4l-dev v4l-utils v4l2ucp  qv4l2 && \
sudo apt-get install -y libgstreamer-plugins-base1.0-dev libgstreamer-plugins-good1.0-dev && \
sudo apt-get install -y libavresample-dev libvorbis-dev libxine2-dev libtesseract-dev && \
sudo apt-get install -y libfaac-dev libmp3lame-dev libtheora-dev libpostproc-dev && \
sudo apt-get install -y libopencore-amrnb-dev libopencore-amrwb-dev && \
sudo apt-get install -y libopenblas-dev libatlas-base-dev libblas-dev && \
sudo apt-get install -y liblapack-dev liblapacke-dev libeigen3-dev gfortran && \
sudo apt-get install -y libhdf5-dev protobuf-compiler nano && \
sudo apt-get install -y libprotobuf-dev libgoogle-glog-dev libgflags-dev
```

#### Enlarge memory swap
Install dphys-swapfile
```
$ sudo apt-get install dphys-swapfile -y
```

Enlarge the boundary
```
$ sudo nano /sbin/dphys-swapfile
```
___edit/change: CONF_MAXSWAP=2048 to CONF_MAXSWAP=6000___

Give the required memory size
```
$ sudo nano /etc/dphys-swapfile
```
___edit/change: #CONF_SWAPSIZE= to CONF_SWAPSIZE=6000___

reboot afterwards
```
$ sudo reboot
```

after reboot check that you now have enough memory
```
$ free -m
```

#### Make sure your in your home directory
```
$ cd $HOME
```

#### Download openCV

Copy the url to the openCV version you want to install ---> https://opencv.org/releases/
I will be using openCV 4.5.5 for this guide. Change the links to the version you want if different
```
$ wget -O opencv.zip https://github.com/opencv/opencv/archive/4.5.5.zip
$ wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.5.5.zip
```
#### unpack
```
$ unzip opencv.zip
$ unzip opencv_contrib.zip
```

#### move files  
```
$ mv opencv-4.5.5 opencv
$ mv opencv_contrib-4.5.5 opencv_contrib
```
### clean up the zip files
```
$ rm opencv.zip
$ rm opencv_contrib.zip
```
#### Build
```
$ cd ~/opencv
$ mkdir build
$ cd build
```
#### CMAKE

Things you may want to change...

-D BUILD_opencv_python2=OFF
<br>
This is set to off so it will only build on python 3. Feel free to change this to 'ON' if you want to also have openCV work in python2.

-D PYTHON3_PACKAGES_PATH=$(***python3*** -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())")
<br>
This grabs the python3 install location. If you have another version of python installed and its not set to exec using python3 but rather its full ver name like python3.9 then change it to that name insted of python3.

Feel free to change, add, or remove things you need for your build.

```
$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
-D EIGEN_INCLUDE_PATH=/usr/include/eigen3 \
-D WITH_OPENCL=OFF \
-D WITH_CUDA=ON \
-D CUDA_ARCH_BIN=5.3 \
-D CUDA_ARCH_PTX="" \
-D WITH_CUDNN=ON \
-D WITH_CUBLAS=ON \
-D ENABLE_FAST_MATH=ON \
-D CUDA_FAST_MATH=ON \
-D OPENCV_DNN_CUDA=ON \
-D ENABLE_NEON=ON \
-D WITH_QT=OFF \
-D WITH_OPENMP=ON \
-D BUILD_TIFF=ON \
-D WITH_FFMPEG=ON \
-D WITH_GSTREAMER=ON \
-D WITH_TBB=ON \
-D BUILD_TBB=ON \
-D BUILD_TESTS=OFF \
-D WITH_EIGEN=ON \
-D WITH_V4L=ON \
-D WITH_LIBV4L=ON \
-D OPENCV_ENABLE_NONFREE=ON \
-D INSTALL_C_EXAMPLES=OFF \
-D INSTALL_PYTHON_EXAMPLES=OFF \
-D BUILD_opencv_python3=ON \
-D BUILD_opencv_python2=OFF \
-D PYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D BUILD_EXAMPLES=OFF ..
```


#### Make
```
$ make -j $(nproc)
```
#### remove the old packages
```
$ sudo rm -r /usr/include/opencv4/opencv2
```

#### Install openCV
```
sudo -H make install
```
#### Congfig
```
$ sudo ldconfig
```
#### Test that it installed
Make sure to use the correct python3 command if you have installed to a diffrent version of python.
```
$ python3 -c 'import cv2 as cv; print("openCV version:",cv.__version__,"CudaEnabledDeviceCount",cv.cuda.getCudaEnabledDeviceCount())'
```
#### cleaning (frees 300 MB)
```
$ make clean
$ sudo apt-get update
```
#### Goto home directory
```
$ cd $HOME
```

#### Remove build stuff
```
$ sudo rm -rf opencv
$ sudo rm -rf opencv_contrib
```
#### Remove the dphys-swapfile now
Note: if your going to be building other stuff like Torch or TorchVision leave the swap your going to need it.
```
$ sudo /etc/init.d/dphys-swapfile stop
$ sudo apt-get remove --purge dphys-swapfile
```



###### END OF DOCUMENT
