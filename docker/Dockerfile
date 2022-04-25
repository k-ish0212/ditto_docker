# TensorFlow (from [3]) ----------------
ARG UBUNTU_VERSION=20.04
ARG B=1
ARG ARCH=
ARG CUDA=11.3
FROM nvidia/cudagl:${CUDA}.0-base-ubuntu${UBUNTU_VERSION} as base

# ARCH and CUDA are specified again because the FROM directive resets ARGs
# (but their default value is retained if set previously)
ARG ARCH
ARG CUDA
ARG CUDNN=8.2.0_0
ARG CUDNN_MAJOR_VERSION=8
ARG LIB_DIR_PREFIX=x86_64
# ARG LIBNVINFER=7.1.3-1
# ARG LIBNVINFER_MAJOR_VERSION=7
# RUN apt-get update && apt-get install -y libcudnn8==

ENV DEBIAN_FRONTEND=noninteractive
# Needed for string substitution
SHELL ["/bin/bash", "-c"]

# RUN apt-get update
# RUN apt-get install -y python3 python3-pip

# RUN pip3 install jupyter
# RUN pip3 install numpy pandas
# RUN pip3 install torch torchvision

# ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cuda-command-line-tools-${CUDA/./-} \
        libcublas-${CUDA/./-} \
        cuda-nvrtc-${CUDA/./-} \
        libcufft-${CUDA/./-} \
        libcurand-${CUDA/./-} \
        libcusolver-${CUDA/./-} \
        libcusparse-${CUDA/./-} \
        curl \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libzmq3-dev \
        pkg-config \
        software-properties-common \
        unzip \
        wget \
        git \
        vim \
        python3-pip \
        lsb-release \
        gnupg \
        avahi-autoipd \
        avahi-daemon \
        avahi-utils


# For CUDA profiling, TensorFlow requires CUPTI.
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:/usr/local/cuda/lib64:$LD_LIBRARY_PATH

# Link the libcuda stub to the location where tensorflow is searching for it and reconfigure
# dynamic linker run-time bindings
RUN ln -s /usr/local/cuda/lib64/stubs/libcuda.so /usr/local/cuda/lib64/stubs/libcuda.so.1 \
    && echo "/usr/local/cuda/lib64/stubs" > /etc/ld.so.conf.d/z-cuda-stubs.conf \
    && ldconfig

RUN apt-get update && apt-get install -y \
    wget git
SHELL ["/bin/bash", "-c"]

# pyenv (from [2]) ----------------
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev \
    libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
    xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
# X window ----------------
RUN apt-get update && apt-get install -y \
    xvfb x11vnc python-opengl icewm
RUN echo 'alias vnc="export DISPLAY=:0; Xvfb :0 -screen 0 1400x900x24 &; x11vnc -display :0 -forever -noxdamage > /dev/null 2>&1 &; icewm-session &"' >> /root/.bashrc

# Copy the helper scripts to the working directory.
COPY ./docker/scripts/. /root/scripts/
RUN chmod -R +x /root/scripts/*

# DL libraries and jupyter ----------------
RUN source /root/.bashrc && \
    pip install setuptools jupyterlab && \
    pip install tensorflow && \
    echo 'alias jl="jupyter lab --ip 0.0.0.0 --port 8888 --NotebookApp.token='' --allow-root &"' >> /root/.bashrc && \
    echo 'alias tb="tensorboard --host 0.0.0.0 --port 6006 --logdir runs &"' >> /root/.bashrc
# pip install pip install torch==1.9.0+cu111 torchvision==0.10.0+cu111 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html && \
    
# utils ----------------
RUN apt-get update && apt-get install -y \
    vim python3-dev

# Setup the Bash shell environment with '/root/.bashrc'.
RUN echo "source /root/scripts/initialize-bash-shell.sh" >> /root/.bashrc

# Force color prompt in terminal.
RUN sed -i 's/#force_color_prompt=yes/force_color_prompt=yes/' /root/.bashrc

# JupyterNotebookのパスワード
RUN mkdir $HOME/.jupyter
COPY jupyter_notebook_config.py $HOME/.jupyter/
ENV PASSWORD password


# install utils3d
RUN mkdir -p /root/Ditto
RUN cd "/root/Ditto" && \
    git clone https://github.com/Steve-Tod/utils3d.git && \
    cd utils3d/ && \
    pip install -e .


RUN pip install --upgrade pip

RUN pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu113
RUN pip install torch-scatter -f https://data.pyg.org/whl/torch-1.11.0+cu113.html

COPY requirements.txt .
# RUN cd "/root/Ditto" && \
RUN pip install -r requirements.txt

RUN apt-get install -y python3-tk
RUN pip install --no-dependencies numpy==1.22.3




ENV PYTHONPATH "/root/Ditto/build/:/usr/local/lib/python3.8/dist-packages/:/root/Ditto/utils3d/"


RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir "/root/work"
WORKDIR "/root"

ENTRYPOINT []

CMD ["/bin/bash"]
