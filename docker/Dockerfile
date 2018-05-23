FROM nvidia/cuda:9.0-base
#FROM nvidia/cuda:9.1-base
MAINTAINER Tom Breuel <tmbdev@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

RUN apt-get -y update

RUN  apt-get -y install sudo lsb-release build-essential curl software-properties-common \
    && echo "deb http://packages.cloud.google.com/apt cloud-sdk-$(lsb_release -c -s) main" \
           >> /etc/apt/sources.list.d/google-cloud-sdk.list \
    && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key  add - \
    && apt-get update -y && apt-get dist-upgrade -y && apt-get -y install apt-utils \
    && apt-get -y install locales && locale-gen en_US.UTF-8 && dpkg-reconfigure locales

RUN apt-get -y install wget tightvncserver tmux rxvt \
    xterm mlterm imagemagick firefox blackbox imagemagick \
    vim-gtk gnome-terminal i3 chromium-browser git mercurial lynx daemon

RUN apt-get -y install nginx

RUN cd /tmp \
    && wget --quiet -nd https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh \
    && bash ./Miniconda2-latest-Linux-x86_64.sh -b -p /opt/conda \
    && ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh \
    && ln -s /opt/conda/bin/conda /usr/bin/conda

RUN apt-get install -y redis-tools

RUN conda install git
RUN conda install numpy
RUN conda install scipy
RUN conda install msgpack
RUN conda install pyzmq
RUN conda install jupyter
RUN conda install scikit-image
RUN conda install scikit-learn
RUN conda install redis
RUN conda install Pillow
RUN conda install pytorch torchvision cuda90 -c pytorch

RUN conda install -c conda-forge google-cloud-storage

RUN conda install pip \
    && ln -s /opt/conda/bin/pip /usr/bin/pip

RUN pip install git+git://github.com/NVLabs/dlinputs
RUN pip install git+git://github.com/NVLabs/dltrainers
RUN pip install git+git://github.com/NVLabs/dlmodels
RUN pip install git+git://github.com/tmbdev/cctc
RUN pip install git+git://github.com/NVLabs/ocrobin
RUN pip install git+git://github.com/NVLabs/ocrodeg
RUN pip install git+git://github.com/NVLabs/ocroline
RUN pip install git+git://github.com/NVLabs/ocrorot
RUN pip install git+git://github.com/NVLabs/ocroseg

RUN conda install simplejson

ENV USER user
ENV HOME /home/$USER
ENV GID 1000
ENV UID 1000

ENV TERM xterm
ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
ENV PATH /opt/conda/bin:/usr/local/nvidia/bin:/usr/local/cuda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

COPY nginx.conf /etc/nginx
EXPOSE 3218

RUN mkdir -p $HOME && groupadd -g $GID -r $USER && useradd --no-log-init -u $UID -r -g $USER $USER

RUN mkdir -p $HOME/.jupyter $HOME/.vnc $HOME/.ssh
ADD jupyter_config/* $HOME/.jupyter/
ADD vnc_config/* $HOME/.vnc/
ADD scripts/* /usr/local/bin/

RUN true \
    && echo ". /opt/conda/etc/profile.d/conda.sh" >> $HOME/.bashrc \
    && echo "conda activate base" >> $HOME/.bashrc \
    && chown -R $USER.$USER $HOME
RUN echo 'user ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers

USER $UID
ENTRYPOINT ["runcmd"]
#CMD /opt/conda/bin/jupyter notebook --allow-root --certfile=$HOME/.jupyter/mycert.pem --keyfile $HOME/.jupyter/mykey.key --port 8888 --no-browser --ip=*
# ENTRYPOINT /opt/conda/bin/ipython

#
#RUN pip install --upgrade jupyterlab && jupyter serverextension enable --py jupyterlab --sys-prefix
#RUN pip install --upgrade dask dask[complete] distributed
#RUN pip install --upgrade google-cloud-storage
#RUN pip install --upgrade youtube-dl
#
##RUN pip install http://download.pytorch.org/whl/cu91/torch-0.3.1-cp27-cp27mu-linux_x86_64.whl
#RUN pip install http://download.pytorch.org/whl/cu90/torch-0.3.1-cp27-cp27mu-linux_x86_64.whl
#RUN pip install torchvision
## RUN apt-get install -y netcat-openbsd
#RUN apt-get install -y netcat-traditional
#RUN apt-get install -y redis-tools
#RUN apt-get install -y webfs
#RUN apt-get install -y iperf3 iozone3
#
