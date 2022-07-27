FROM debian:buster-slim

MAINTAINER trngaje <bilbobaggins@naver.com> 

ENV DEBIAN_FRONTEND=noninteractive

RUN echo "deb http://deb.debian.org/debian buster-backports main" > /etc/apt/sources.list.d/buster-backports.list && \
    apt-get update && apt-get upgrade -y && \
    apt-get install -y --no-install-recommends apt-utils && \
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
RUN mkdir /home/user/tools/devkitbuild
WORKDIR /home/user/tools/devkitbuild

RUN wget https://apt.devkitpro.org/install-devkitpro-pacman
RUN chmod +x ./install-devkitpro-pacman
RUn sudo ./install-devkitpro-pacman
RUN echo "export DEVKITPRO=/opt/devkitpro" >> /home/user/.bashrc
RUN echo "export DEVKITARM=/opt/devkitpro/devkitARM" >> /home/user/.bashrc
RUN echo "export PATH=$PATH:/opt/devkitpro/devkitARM/bin" >> /home/user/.bashrc
#RUN source ~/.profile

ENV DEVKITPRO /opt/devkitpro
ENV DEVKITARM /opt/devkitpro/devkitARM

RUN sudo dkp-pacman -Sy
RUN sudo dkp-pacman -Syu
RUN sudo dkp-pacman -S 3ds-dev --noconfirm 


#Make armips
WORKDIR /home/user/tools
RUN git clone --recursive https://github.com/Kingcom/armips.git
RUN mkdir /home/user/tools/armips/build
WORKDIR /home/user/tools/armips/build
RUN cmake ../
RUN make
RUN sudo cp armips /usr/local/bin

#Make ctrtool / makerom
WORKDIR /home/user/tools
RUN git clone https://github.com/profi200/Project_CTR.git
WORKDIR /home/user/tools/Project_CTR
RUN make
RUN sudo cp /home/user/tools/Project_CTR/ctrtool/bin/ctrtool /usr/local/bin
RUN sudo cp /home/user/tools/Project_CTR/makerom/bin/makerom /usr/local/bin

#Get most updated version of ctrulib
WORKDIR /home/user/tools
#RUN git clone https://github.com/smealum/ctrulib.git
RUN git clone https://github.com/devkitPro/libctru.git

#WORKDIR /home/user/tools/ctrulib/libctru
WORKDIR /home/user/tools/libctru/libctru

RUN make
RUN export DEVKITPRO=/opt/devkitpro
RUN export DEVKITARM=/opt/devkitpro/devkitARM
RUN export PATH=$PATH:/opt/devkitpro/devkitARM
RUN sudo -E make install

#Get most updated version of citro3d
WORKDIR /home/user/tools
RUN git clone https://github.com/fincs/citro3d.git
WORKDIR /home/user/tools/citro3d
RUN make
RUN export DEVKITPRO=/opt/devkitpro
RUN export DEVKITARM=/opt/devkitpro/devkitARM
RUN export PATH=$PATH:/opt/devkitpro/devkitARM
RUN sudo -E make install


WORKDIR /home/user/tools
RUN git clone https://github.com/TuxSH/firmtool.git
WORKDIR /home/user/tools/firmtool
RUN sudo python setup.py install
RUN sudo python3 setup.py install

#Get Luma3DS and build
WORKDIR /home/user/tools
RUN git clone --recursive https://github.com/AuroraWright/Luma3DS.git
WORKDIR /home/user/tools/Luma3DS
RUN make

#Get latest GodMode9
WORKDIR /home/user/tools
RUN git clone --recursive https://github.com/d0k3/GodMode9.git
WORKDIR /home/user/tools/GodMode9
RUN make



#Get homemenu hax
WORKDIR /home/user/tools
RUN git clone https://github.com/yellows8/3ds_homemenuhax.git

#Get homemenu PKSM
WORKDIR /home/user/tools
RUN git clone https://github.com/BernardoGiordano/PKSM.git


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
