FROM arm64v8/python:3.7.9

RUN mkdir /app \
&& apt-get update -y \
&& apt-get install -y \
    build-essential \
    libssl-dev libffi-dev \
    python3-dev python3-pip \
    libsasl2-dev libldap2-dev \
&& rm -rf /var/lib/apt/lists/*            

# from https://discuss.streamlit.io/t/raspberry-pi-streamlit/2900/35
# Download the package repo keyring
RUN wget https://apache.bintray.com/arrow/ubuntu/apache-arrow-archive-keyring-latest-focal.deb

# Install it
RUN apt install ./apache-arrow-archive-keyring-latest-focal.deb

# Update apt
RUN apt update

# Install apache arrow headers pegged at version 1.0.1-1
RUN apt install libarrow-dev=1.0.1-1 libarrow-python-dev=1.0.1-1
RUN apt-mark hold libarrow-dev libarrow-python-dev

RUN APYARROW_CMAKE_OPTIONS="-DARROW_ARMV8_ARCH=armv8-a" pip install pyarrow
# RUN pip install apache-superset

ENTRYPOINT ["superset"]