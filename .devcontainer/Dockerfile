ARG VERSION=20.12
FROM nvcr.io/nvidia/pytorch:${VERSION}-py3

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 847005AE619067D5

## tzdata
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Seoul
RUN apt-get update && \
    apt-get install -y tzdata && \
    ln -fs /usr/share/zoneinfo/Asia/Seoul /etc/localtime && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    rm -rf /var/lib/apt/lists/*
    
RUN apt-get update && \
    apt-get install -qq -y \
    wget \
    git \
    build-essential \
	curl \
	bash-completion \
	htop \
	cmake \
    sudo \
    libgl1

# Install nvtop
RUN git clone -b 2.0.4 https://github.com/Syllo/nvtop.git && \
    mkdir -p nvtop/build && cd nvtop/build && \
    cmake .. && \
    make && \
    make install && \
    cd ../.. && \
    rm -rf nvtop

ARG USERNAME

# Create the user
RUN groupadd $USERNAME \
    && useradd -g $USERNAME -m $USERNAME \
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME

# Install Python packages. Python packages must be installed as user permission.
RUN pip install \
	torch==1.11.0 \
	torchvision>=0.9.1 \
    imageio \
    imageio-ffmpeg \
    matplotlib \
    configargparse \
    tensorboard>=2.0 \
    tqdm \
    opencv-python

ENV SHELL /bin/bash