# ==================================================================
# module list
# ------------------------------------------------------------------
# python        3.6    (apt)
# pytorch       latest (pip)
# theano        latest (git)
# lasagne       latest (git)
# ==================================================================

FROM ubuntu:18.04
ENV LANG C.UTF-8
RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    GIT_CLONE="git clone --depth 10" && \

    rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list && \

    apt-get update && \

# ==================================================================
# tools
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        build-essential \
        apt-utils \
        ca-certificates \
        wget \
        git \
        vim \
        libssl-dev \
        curl \
        unzip \
        unrar \
        && \

    $GIT_CLONE https://github.com/Kitware/CMake ~/cmake && \
    cd ~/cmake && \
    ./bootstrap && \
    make -j"$(nproc)" install && \

# ==================================================================
# python
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        software-properties-common \
        && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        python3.6 \
        python3.6-dev \
        python3-distutils-extra \
        && \
    wget -O ~/get-pip.py \
        https://bootstrap.pypa.io/get-pip.py && \
    python3.6 ~/get-pip.py && \
    ln -s /usr/bin/python3.6 /usr/local/bin/python3 && \
    ln -s /usr/bin/python3.6 /usr/local/bin/python && \
    $PIP_INSTALL \
        setuptools \
        && \
    $PIP_INSTALL \
        numpy \
        scipy \
        pandas \
        cloudpickle \
        scikit-learn \
        matplotlib \
        Cython \
        && \

# ==================================================================
# pytorch
# ------------------------------------------------------------------

    $PIP_INSTALL \
        future \
        numpy \
        protobuf \
        enum34 \
        pyyaml \
        typing \
    	torch \
        && \
    $PIP_INSTALL \
    "git+https://github.com/pytorch/vision.git" && \
    $PIP_INSTALL \
        torch_nightly -f \
        https://download.pytorch.org/whl/nightly/cpu/torch_nightly.html \
        && \

# ==================================================================
# theano
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libblas-dev \
        && \

    $PIP_INSTALL \
        https://github.com/Theano/Theano/archive/master.zip \
        && \

# ==================================================================
# lasagne
# ------------------------------------------------------------------

    $GIT_CLONE https://github.com/Lasagne/Lasagne ~/lasagne && \
    cd ~/lasagne && \
    $PIP_INSTALL \
        . && \

# ==================================================================
# config & cleanup
# ------------------------------------------------------------------

    ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*
