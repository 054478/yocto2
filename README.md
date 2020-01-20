# set the base image
FROM ubuntu:16.04

#set
USER root

# update sources list
RUN apt-get clean
RUN apt-get update

# set bash
RUN ln -sf /bin/bash /bin/sh

# set account
RUN useradd -m developer
RUN apt-get install -qy sudo
RUN usermod -G sudo developer
RUN echo "developer ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

USER developer
ENV HOME=/home/developer
WORKDIR $HOME

# install command
RUN sudo apt-get install -qy gawk
RUN sudo apt-get install -qy wget
RUN sudo apt-get install -qy git-core
RUN sudo apt-get install -qy diffstat
RUN sudo apt-get install -qy unzip
RUN sudo apt-get install -qy texinfo
RUN sudo apt-get install -qy gcc-multilib
RUN sudo apt-get install -qy build-essential
RUN sudo apt-get install -qy chrpath
RUN sudo apt-get install -qy socat
RUN sudo apt-get install -qy cpio
RUN sudo apt-get install -qy python
RUN sudo apt-get install -qy python3
RUN sudo apt-get install -qy python3-pip
RUN sudo apt-get install -qy python3-pexpect
RUN sudo apt-get install -qy xz-utils
RUN sudo apt-get install -qy debianutils
RUN sudo apt-get install -qy iputils-ping
RUN sudo apt-get install -qy python3-git
RUN sudo apt-get install -qy python3-jinja2
RUN sudo apt-get install -qy libegl1-mesa
RUN sudo apt-get install -qy libsdl1.2-dev
RUN sudo apt-get install -qy pylint3
RUN sudo apt-get install -qy vim

# set utf-8
RUN sudo apt-get install -qy locales
RUN sudo dpkg-reconfigure locales 
RUN sudo locale-gen en_US.UTF-8
RUN sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG=en_US.UTF-8

# poky install
RUN git clone git://git.yoctoproject.org/poky
WORKDIR ./poky
RUN git checkout tags/yocto-2.5 -b my-yocto-2.5
#RUN source oe-init-build-env

# cleanup
RUN sudo apt-get -qy autoremove
