# from https://gist.github.com/heavyinfo/04e1326bb9bed9cecb19c2d603c8d521
FROM arm64v8/ubuntu:bionic

ENV ARROW_HOME=/usr/local/lib
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
ENV PYARROW_WITH_FLIGHT=1
ENV PYARROW_WITH_GANDIVA=1
ENV PYARROW_WITH_ORC=1
ENV PYARROW_WITH_PARQUET=1
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
    && rm -rf /var/lib/apt/lists/*
RUN pip3 install Cython
RUN pip3 install numpy
RUN pip3 install six numpy pandas pytest psutil            

WORKDIR /build
ADD https://github.com/apache/arrow/archive/apache-arrow-0.15.1.zip . 
RUN ls -l /build
RUN unzip apache-arrow-0.15.1.zip
WORKDIR /build/arrow-apache-arrow-0.15.1/cpp/release

RUN cmake -DCMAKE_INSTALL_PREFIX=$ARROW_HOME \       
      -DCMAKE_INSTALL_LIBDIR=lib  \
      -DARROW_FLIGHT=ON \
      -DARROW_GANDIVA=ON  \
      -DARROW_ORC=ON  \
      -DARROW_WITH_BZ2=ON \       
      -DARROW_WITH_ZLIB=ON  \
      -DARROW_WITH_ZSTD=ON  \
      -DARROW_WITH_LZ4=ON \       
      -DARROW_WITH_SNAPPY=ON  \      
      -DARROW_WITH_BROTLI=ON  \       
      -DARROW_PARQUET=ON  \
      -DARROW_PYTHON=ON \
      -DARROW_PLASMA=ON \
      -DARROW_CUDA=OFF \
      -DARROW_BUILD_TESTS=ON  \
      -DPYTHON_EXECUTABLE=/usr/bin/python3  \
      ..
RUN make -j4
RUN make install

WORKDIR /build/arrow-apache-arrow-0.15.1/cpp/python

RUN pip3 install -r requirements.txt


RUN python3 setup.py build_ext --inplace
RUN python3 setup.py install