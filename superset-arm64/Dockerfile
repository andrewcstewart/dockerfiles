# from https://gist.github.com/heavyinfo/04e1326bb9bed9cecb19c2d603c8d521
FROM arm64v8/ubuntu:bionic

ENV ARROW_HOME=/usr/local/lib
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
ENV PYARROW_WITH_FLIGHT=1
ENV PYARROW_WITH_GANDIVA=0
ENV PYARROW_WITH_ORC=1
ENV PYARROW_WITH_PARQUET=0
ENV PYARROW_WITH_CUDA=0
ENV PYARROW_WITH_PLASMA=1

RUN apt-get update -y \
    && apt-get install -y \
    libjemalloc-dev \
    libboost-dev \
    libboost-filesystem-dev \
    libboost-system-dev \
    libboost-regex-dev \
    python3-dev \
    python3-pip \
    cython \
    autoconf \
    flex \
    bison \
    libssl-dev \
    curl \
    cmake \
    unzip \
    # llvm \
    && rm -rf /var/lib/apt/lists/*
RUN pip3 install Cython
RUN pip3 install numpy
RUN pip3 install six numpy pandas pytest psutil            

WORKDIR /build
ADD https://github.com/apache/arrow/archive/apache-arrow-0.15.1.zip . 
RUN unzip apache-arrow-0.15.1.zip
# RUN ls -l /build/apache-arrow-0.15.1/cpp
RUN ls -l /build/arrow-apache-arrow-0.15.1/cpp
# WORKDIR /build/apache-arrow-0.15.1/cpp/release
WORKDIR /build/arrow-apache-arrow-0.15.1/cpp/release

RUN cmake -DCMAKE_INSTALL_PREFIX=$ARROW_HOME \       
      -DCMAKE_INSTALL_LIBDIR=lib  \
      -DARROW_FLIGHT=ON \
      -DARROW_GANDIVA=OFF  \
      -DARROW_ORC=ON  \
      -DARROW_WITH_BZ2=ON \       
      -DARROW_WITH_ZLIB=ON  \
      -DARROW_WITH_ZSTD=ON  \
      -DARROW_WITH_LZ4=ON \       
      -DARROW_WITH_SNAPPY=ON  \      
      -DARROW_WITH_BROTLI=ON  \       
      -DARROW_PARQUET=OFF  \
      -DARROW_PYTHON=ON \
      -DARROW_PLASMA=ON \
      -DARROW_CUDA=OFF \
      -DARROW_BUILD_TESTS=ON  \
      -DPYTHON_EXECUTABLE=/usr/bin/python3  \
      ..
RUN make -j4
RUN make install

# WORKDIR /build/apache-arrow-0.15.1/python
RUN ls -l /build/arrow-apache-arrow-0.15.1/cpp/release
RUN ls -l /build/arrow-apache-arrow-0.15.1/
WORKDIR /build/arrow-apache-arrow-0.15.1/python
RUN ls -l /build/arrow-apache-arrow-0.15.1/python

RUN pip3 install -r requirements.txt


RUN python3 setup.py build_ext --inplace
RUN python3 setup.py install

RUN mkdir /app \
&& apt-get update -y \
&& apt-get install -y \
    build-essential \
    libssl-dev libffi-dev \
    python3-dev python3-pip \
    libsasl2-dev libldap2-dev \
    git-all \
&& rm -rf /var/lib/apt/lists/*            

# from https://discuss.streamlit.io/t/raspberry-pi-streamlit/2900/35
# Download the package repo keyring
# RUN wget https://apache.bintray.com/arrow/ubuntu/apache-arrow-archive-keyring-latest-focal.deb

# # Install it
# RUN apt install ./apache-arrow-archive-keyring-latest-focal.deb

# # Update apt
# RUN apt update

# # Install apache arrow headers pegged at version 1.0.1-1
# RUN apt install libarrow-dev=1.0.1-1 libarrow-python-dev=1.0.1-1
# RUN apt-mark hold libarrow-dev libarrow-python-dev

# RUN APYARROW_CMAKE_OPTIONS="-DARROW_ARMV8_ARCH=armv8-a" pip install pyarrow
RUN pip3 install apache-superset

ENTRYPOINT ["superset"]