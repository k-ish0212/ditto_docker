# TensorFlow (from [3]) ----------------
ARG UBUNTU_VERSION=20.04

ARG ARCH=
ARG CUDA=11.3
FROM nvidia/cuda${ARCH:+-$ARCH}:${CUDA}.0-base-ubuntu${UBUNTU_VERSION} as base
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

RUN apt-get update
RUN apt-get install -y python3 python3-pip

RUN pip3 install jupyter
RUN pip3 install numpy pandas
RUN pip3 install torch torchvision

ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs

RUN mkdir "/root/work"
WORKDIR "/root/work"

CMD ["jupyter", "notebook", "--ip=*", "--no-browser"]


# ARG NB_USER="ditto"
# ARG NB_UID="1000"



# RUN echo "auth requisite pam_deny.so" >> /etc/pam.d/su
# RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER

# USER $NB_UID
# RUN mkdir "/home/$NB_USER/work"

# WORKDIR "/home/$NB_USER/work"

# CMD ["jupyter", "notebook", "--ip=*", "--no-browser"]
