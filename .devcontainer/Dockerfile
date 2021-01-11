FROM nvcr.io/nvidia/l4t-base:r32.3.1

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get install -yq wget curl git build-essential vim sudo

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID --shell /bin/bash -m $USERNAME
RUN echo $USERNAME:$USERNAME | chpasswd && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# opencv
RUN apt-get install -y \
    libglew-dev \
    libtiff5-dev \
    zlib1g-dev \
    libjpeg-dev \
    libpng-dev \
    # libjasper-dev \
    libavcodec-dev \
    libavformat-dev \
    libavutil-dev \
    libpostproc-dev \
    libswscale-dev \
    libeigen3-dev \
    libtbb-dev \
    libgtk2.0-dev \
    cmake \
    pkg-config

# GStreamer
RUN apt install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev unzip

WORKDIR /tmp
ENV CV_VERSION 4.2.0
RUN wget https://github.com/opencv/opencv/archive/$CV_VERSION.zip -O opencv.zip && \
    unzip -q opencv.zip && \
    wget https://github.com/opencv/opencv_contrib/archive/$CV_VERSION.zip -O opencv_contrib.zip && \
    unzip -q opencv_contrib.zip
RUN mkdir opencv-$CV_VERSION/build
WORKDIR /tmp/opencv-$CV_VERSION/build
RUN cmake \
        -DCMAKE_BUILD_TYPE=Release \
        -DBUILD_PNG=OFF \
        -DBUILD_TIFF=OFF \
        -DBUILD_TBB=OFF \
        -DBUILD_JPEG=OFF \
        -DBUILD_JASPER=OFF \
        -DBUILD_ZLIB=OFF \
        -DBUILD_EXAMPLES=OFF \
        -DBUILD_opencv_java=OFF \
        -DBUILD_opencv_python2=OFF \
        -DBUILD_opencv_python3=ON \
        -DENABLE_PRECOMPILED_HEADERS=OFF \
        -DWITH_OPENCL=OFF \
        -DWITH_OPENMP=OFF \
        -DWITH_FFMPEG=ON \
        -DWITH_GSTREAMER=ON \
        -DWITH_GSTREAMER_0_10=OFF \
        -DWITH_CUDA=ON \
        -DWITH_GTK=ON \
        -DWITH_VTK=OFF \
        -DWITH_TBB=ON \
        -DWITH_1394=OFF \
        -DWITH_OPENEXR=OFF \
        -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda \
        -DCUDA_ARCH_BIN=5.3 \
        -DCUDA_ARCH_PTX="" \
        -DINSTALL_C_EXAMPLES=OFF \
        -DINSTALL_TESTS=OFF \
        -DOPENCV_EXTRA_MODULES_PATH=/tmp/opencv_contrib-$CV_VERSION/modules ..
RUN make -j4 && make install
RUN rm /tmp/opencv.zip && \
    rm /tmp/opencv_contrib.zip && \
    rm -r /tmp/opencv-$CV_VERSION && \
    rm -r /tmp/opencv_contrib-$CV_VERSION

RUN rm -rf /var/lib/apt/lists/*
