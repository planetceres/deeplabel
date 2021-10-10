# https://github.com/ufoym/deepo
FROM ufoym/deepo:all


# Install packages without prompting the user to answer any questions
ENV DEBIAN_FRONTEND noninteractive 
ENV PB_REL="https://github.com/protocolbuffers/protobuf/releases"

# Install packages
RUN apt-get update && apt-get install -qq -y \
    locales \
    lsb-release \
    mesa-utils \
    git \
    subversion \
    nano \
    terminator \
    xterm \
    wget \
    curl \
    htop \
    libssl-dev \
    dbus-x11 \
    software-properties-common \
    build-essential \
    qt5-default \
    cmake \
    pkg-config \
    libjpeg-dev \
    libtiff5-dev \
    libpng-dev \
    autoconf \
    automake \
    libtool \
    make \
    g++ \
    unzip \
    gdb valgrind && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/opencv/opencv
RUN git clone https://github.com/opencv/opencv_contrib
RUN cd opencv && git checkout 4.5.3 && cd ../
RUN cd opencv_contrib && git checkout 4.5.3 && cd ../

RUN cd opencv && \
    mkdir build && cd build && \
    cmake .. -DBUILD_TESTS=OFF -DBUILD_PERF_TESTS=OFF -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules && \
    make -j8  && \
    sudo make install

# https://github.com/protocolbuffers/protobuf/blob/master/src/README.md
RUN git clone https://github.com/protocolbuffers/protobuf.git && \
    cd protobuf && \
    git submodule update --init --recursive && \
    ./autogen.sh && \
    ./configure && \
    make -j8 && \
    make check && \
    make install && \
    ldconfig

RUN which -a protoc
RUN mv /usr/bin/protoc /usr/bin/protoc1
RUN ln -sf /usr/local/bin/protoc /usr/bin/protoc
RUN /usr/bin/protoc --version
RUN /usr/local/bin/protoc --version


WORKDIR /root
RUN git clone https://github.com/jveitchmichaelis/deeplabel.git
WORKDIR /root/deeplabel
RUN protoc --proto_path ./src/proto --cpp_out ./src/proto feature.proto example.proto

RUN git submodule update --init --recursive && \
    qmake -makefile -o Makefile DeepLabel.pro && \
    make -j8 

RUN ln -sf /root/deeplabel/release/deeplabel /usr/bin/deeplabel

WORKDIR /root
