FROM python:3.7-slim

LABEL maintainer="rodrigokanehisa@gmail.com"

RUN apt-get update \
    && apt-get install -y \
        build-essential \
        cmake \
        git \
        wget \
        unzip \
        yasm \
        pkg-config \
        libswscale-dev \
        libtbb2 \
        libtbb-dev \
        libjpeg-dev \
        libpng-dev \
        libtiff-dev \
        libavformat-dev \
        libpq-dev \
    && rm -rf /var/lib/apt/lists/*

RUN pip install numpy

WORKDIR /
ENV OPENCV_VERSION="4.1.1"
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip -O opencv.zip \
  && unzip opencv.zip \
  && wget https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.zip -O opencv_contrib.zip \
  && unzip opencv_contrib \
  && mkdir /opencv-${OPENCV_VERSION}/cmake_binary \
  && cd /opencv-${OPENCV_VERSION}/cmake_binary \
  && cmake -DOPENCV_EXTRA_MODULES_PATH=/opencv_contrib-${OPENCV_VERSION}/modules \
    -DBUILD_TIFF=ON \
    -DBUILD_opencv_java=OFF \
    -DWITH_CUDA=OFF \
    -DWITH_OPENGL=ON \
    -DWITH_OPENCL=ON \
    -DWITH_IPP=ON \
    -DWITH_TBB=ON \
    -DWITH_EIGEN=ON \
    -DWITH_V4L=ON \
    -DBUILD_TESTS=OFF \
    -DOPENCV_ENABLE_NONFREE:BOOL=ON \
    -DBUILD_PERF_TESTS=OFF \
    -DCMAKE_BUILD_TYPE=RELEASE \
    -DBUILD_opencv_python3=ON \
    -DCMAKE_INSTALL_PREFIX=$(python3.7 -c "import sys; print(sys.prefix)") \
    -DPYTHON_EXECUTABLE=$(which python3.7) \
    -DPYTHON_INCLUDE_DIR=$(python3.7 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
    -DPYTHON_PACKAGES_PATH=$(python3.7 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") .. \
  && make install \
  && rm /opencv.zip \
  && rm /opencv_contrib.zip \
  && rm -r /opencv-${OPENCV_VERSION} \
  && rm -r /opencv_contrib-${OPENCV_VERSION}
