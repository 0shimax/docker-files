# # operate in hostOS before make docker image

# # install Cuda Toolkit
# https://developer.nvidia.com/cuda-downloads
# wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
# sudo dpkg -i cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
# sudo apt-get update
# sudo apt-get install -y cuda
# sudo reboot
# # create nvidia-uvm
# /usr/local/cuda/samples/1_Utilities/deviceQuery/deviceQuery
# # setting .bashrc
# export CUDA_PATH=/usr/local/cuda-8.0
# export PATH=/usr/local/cuda-8.0/bin:${PATH}
# export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}
# export CPATH=$CUDA_PATH/include:$CPATH
# export LIBRARY_PATH=$CUDA_PATH/lib64:$LD_LIBRARY_PATH
# export LD_LIBRARY_PATH=$CUDA_PATH/lib64:$LD_LIBRARY_PATH
# source .bashrc

# # install docker
# apt-get update
# apt-get install apt-transport-https ca-certificates
# apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
# echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee /etc/apt/sources.list.d/docker.list > dev/null
# apt-get update
# # apt-cache policy docker-engine
# # apt-get install docker-engine
# wget -qO- https://get.docker.com/ | sh
# usermod -aG docker shimada
# service docker restart
# exit and relogin
# docker run hello-world

# # download and install nvidia-docker
# wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.0-rc.3/nvidia-docker_1.0.0.rc.3-1_amd64.deb
# dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb

# # Test nvidia-smi
## for cuda toolkit==8.0
# nvidia-docker run --rm nvidia/cuda nvidia-smi
## for cuda toolkit==7.5
# nvidia-docker run --rm nvidia/cuda:7.5-devel nvcc --version
# # do this command to create nvidia-uvm
# nvidia-modprobe -u -c=0


##############################################################################
# base
##############################################################################
FROM nvidia/cuda:8.0  # nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04

# locale for tokyo
RUN cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# enable apt multiverse repository
RUN apt-get update && \
    # apt-get upgrade && \
    apt-get install -y software-properties-common python-software-properties && \
    add-apt-repository multiverse

# Disable frontend
ENV DEBIAN_FRONTEND=noninteractive

# Install wget and build-essential
RUN apt-get update && apt-get install -y \
  build-essential \
  wget \
  linux-generic
  # nvidia-cuda-toolkit \
  # nvidia-361

##############################################################################
# anaconda python
##############################################################################
# Install Anaconda
RUN apt-get update && \
    apt-get install -y wget bzip2 ca-certificates libmysqlclient-dev && \
    rm -rf /var/lib/apt/lists/*

RUN wget --quiet https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh && \
    /bin/bash Anaconda3-4.2.0-Linux-x86_64.sh -b -p /opt/conda && \
    rm Anaconda3-4.2.0-Linux-x86_64.sh

ENV PATH /opt/conda/bin:$PATH
RUN pip install -U pip

RUN conda update conda
RUN conda update anaconda

# for jupyter notebook
EXPOSE 8888

##############################################################################
# chainer
##############################################################################
RUN pip install -U -I setuptools
RUN pip install chainer

##############################################################################
# opencv
##############################################################################
RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y libgtk2.0-dev
RUN conda install -y -c https://conda.anaconda.org/menpo opencv3
