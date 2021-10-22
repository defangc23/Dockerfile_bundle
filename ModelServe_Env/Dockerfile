# ==================================================================
# module list
# ------------------------------------------------------------------
# miniconda        3.8
# ray              1.3.0
# ==================================================================

FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu16.04
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
        ffmpeg libsm6 libxext6 \
        && \

    $GIT_CLONE https://github.com/Kitware/CMake ~/cmake && \
    cd ~/cmake && \
    ./bootstrap && \
    make -j"$(nproc)" install && \


# ==================================================================
# miniconda
# ------------------------------------------------------------------

    wget \
    https://repo.anaconda.com/miniconda/Miniconda3-py38_4.8.2-Linux-x86_64.sh && \
    bash Miniconda3-py38_4.8.2-Linux-x86_64.sh -p /miniconda -b && \
    rm -rf Miniconda3-py38_4.8.2-Linux-x86_64.sh
ENV PATH=/miniconda/bin/:${PATH}
RUN conda update -y conda && \
    conda init bash && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/ && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/linux-64/ && \
    conda config --set show_channel_urls yes && \
    conda config --set auto_activate_base false
RUN conda create -y -n AlgoExample python=3.6.12
SHELL ["conda", "run", "-n", "AlgoExample", "/bin/bash", "-c"]
RUN pip install -U ray==1.3.0 && \
    pip install pydantic && \
    pip install uvicorn && \
    pip install starlette && \
    pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/ && \
    pip config set global.trusted-host mirrors.aliyun.com && \


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


ENV NOTVISIBLE="in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile
EXPOSE 22
