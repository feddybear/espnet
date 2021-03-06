FROM ubuntu:18.04
LABEL maintainer "Nelson Yalta <nyalta21@gmail.com>"

ARG DOCKER_VER
ENV DOCKER_BUILT_VER ${DOCKER_VER}}

ARG NUM_BUILD_CORES=8
ENV NUM_BUILD_CORES ${NUM_BUILD_CORES}

RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get -y install --no-install-recommends \
        automake \
        autoconf \
        apt-utils \
        bc \
        build-essential \
        ca-certificates \
        cmake \
        curl \
        flac \
        ffmpeg \
        gawk \
        gfortran \
        git \
        libtool \
        libsndfile1-dev \
        python2.7 \
        python3 \
        python3-pip \
        python3-dev \
        sox \
        subversion \
        unzip \
        wget \
        zip \
        zlib1g-dev \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Upgrade pip and install jupyter
RUN pip3 -q install pip --upgrade && \
    pip3 install --upgrade nbconvert && \
    pip3 install jupyterlab

# Install Kaldi
RUN git clone https://github.com/kaldi-asr/kaldi

RUN cd /kaldi/tools && \
    ./extras/install_mkl.sh -sp debian intel-mkl-64bit-2019.2-057 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    make all && \
    rm -r openfst-*/src && \
    ./extras/install_beamformit.sh && \
    ./extras/install_irstlm.sh && \
    cd /kaldi/src && \
    ./configure --shared --use-cuda=no && \
    make depend -j${NUM_BUILD_CORES} && \
    make -j${NUM_BUILD_CORES} && \
    find /kaldi/src -name "*.o" -exec rm -f {} \; && \
    find /kaldi/src -name "*.o" -exec rm -f {} \;

RUN wget --tries=3 "https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh" -O miniconda.sh && \
    bash miniconda.sh -b -p /miniconda && \
    rm miniconda.sh

WORKDIR /
