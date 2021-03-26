# ==================================================================
# module list
# ------------------------------------------------------------------
# python           3.8
# ray serve        1.2.0
# miniconda        3.8
# ==================================================================

FROM nvidia/cuda:10.0-cudnn7-runtime-ubuntu18.04
ENV LANG C.UTF-8
RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \

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

# ==================================================================
# python
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        software-properties-common \
        && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        python3.8 \
        python3.8-dev \
        python3-distutils \
        && \
    wget -O ~/get-pip.py \
        https://bootstrap.pypa.io/get-pip.py && \
    python3.8 ~/get-pip.py && \
    ln -s /usr/bin/python3.8 /usr/local/bin/python3 && \
    ln -s /usr/bin/python3.8 /usr/local/bin/python && \
   pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/ && \
   pip config set global.trusted-host mirrors.aliyun.com && \

# ==================================================================
# ray serve
# ------------------------------------------------------------------

    $PIP_INSTALL \
    ray[serve]==1.2.0 \
    && \

# ==================================================================
# miniconda
# ------------------------------------------------------------------

    wget \
    https://repo.anaconda.com/miniconda/Miniconda3-py38_4.8.2-Linux-x86_64.sh && \
    bash Miniconda3-py38_4.8.2-Linux-x86_64.sh -p /miniconda -b && \
    rm -rf Miniconda3-py38_4.8.2-Linux-x86_64.sh && \

# ==================================================================
# SSH
# ------------------------------------------------------------------

    apt-get update && apt-get install -y openssh-server && \
    mkdir /var/run/sshd && \
    echo 'root:iiisct2019' | chpasswd && \
    sed -i 's/#*PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config && \
    sed -i 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' /etc/pam.d/sshd && \

# ==================================================================
# Last Step ------ config & cleanup
# ------------------------------------------------------------------

    ldconfig && \
    apt-get autoremove -y && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*

ENV PATH=/miniconda/bin:${PATH}
ENV PYTHONPATH=/usr/local/bin/python
RUN conda update -y conda
ENV NOTVISIBLE="in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile
EXPOSE 22
