FROM debian:buster-slim

MAINTAINER trngaje <bilbobaggins@naver.com> 

ENV DEBIAN_FRONTEND=noninteractive

RUN echo "deb http://deb.debian.org/debian buster-backports main" > /etc/apt/sources.list.d/buster-backports.list && \
    apt-get update && apt-get upgrade -y && \
    apt-get install -y --no-install-recommends apt-utils nano && \
    apt-get install -y --no-install-recommends sudo ca-certificates pkg-config curl wget bzip2 xz-utils make bsdtar doxygen gnupg && \
    apt-get install -y --no-install-recommends git git-restore-mtime && \
    apt-get install -y --no-install-recommends cmake/buster-backports zip unzip build-essential tmux python2.7 python-pip python-dev python-setuptools python3 python3-setuptools&& \
    apt-get install -y --no-install-recommends locales && \
    sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
	
#Get dependencies needed for most projects: update as needed for other homebrew projects
#RUN apt-get update && apt-get -y install tmux vim git build-essential curl wget cmake gdb gdbserver emacs netcat python2.7 python-dev sudo socat wget zip bsdmainutils binwalk libarchive-tools gpg


#libarchive-tools gpg

RUN useradd -m user
RUN usermod -s /bin/bash user
ENV SHELL /bin/bash
ENV HOME /home/user
RUN echo "user ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER user

RUN mkdir /home/user/tools

WORKDIR /home/user/tools

#Set up devkitarm
#RUN mkdir /home/user/tools/
WORKDIR /home/user/tools/

RUN sudo mkdir -p /opt/devkitpro
RUN wget "https://wii.leseratte10.de/devkitPro/devkitARM/r45%20(2015)/devkitARM_r45-x86_64-linux.tar.bz2"
RUN sudo tar -xvf devkitARM_r45-x86_64-linux.tar.bz2 -C /opt/devkitpro/
RUN wget https://wii.leseratte10.de/devkitPro/libctru/2015/libctru-1.0.0.tar.gz
RUN tar -xvzf libctru-1.0.0.tar.gz
WORKDIR /home/user/tools/libctru-1.0.0/libctru
ENV DEVKITPRO /opt/devkitpro
ENV DEVKITARM /opt/devkitpro/devkitARM
RUN sudo -E make install
RUN wget https://wii.leseratte10.de/devkitPro/other-stuff/citro3d/citro3d-1.0.0.tar.bz2
RUN sudo tar -xvf citro3d-1.0.0.tar.bz2 -C /opt/devkitpro/libctru/

RUN echo "export DEVKITPRO=/opt/devkitpro" >> /home/user/.bashrc
RUN echo "export DEVKITARM=/opt/devkitpro/devkitARM" >> /home/user/.bashrc
RUN echo "export PATH=$PATH:/opt/devkitpro/devkitARM/bin" >> /home/user/.bashrc
#RUN source ~/.profile








#Setup VIM configurations
RUN echo "imap jj <Esc>" >> ~/.vimrc
RUN echo "set number" >> ~/.vimrc
RUN echo "syntax on" >> ~/.vimrc
RUN echo "set tabstop=4 shiftwidth=4 expandtab" >> ~/.vimrc
RUN echo "LS_COLORS=$LS_COLORS:'di=0;35:' ; export LS_COLORS" >> ~/.bashrc

#Set default editor and startup command
WORKDIR /home/user/
ENV EDITOR vim
CMD tmux
