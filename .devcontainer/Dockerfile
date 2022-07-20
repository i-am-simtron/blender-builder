FROM ubuntu:latest

ARG APT_PROXY="http://172.17.0.1:3142"
ARG USER
ARG GIT_USER
ARG GIT_EMAIL

# [OPTIONAL] Caching APT repos
# 
# Read more here
# https://hub.docker.com/r/sameersbn/apt-cacher-ng
#
# Because this is a developer image, `apt` operations can be repetitive and
# data heavy. I prefer to cache my `apt` packages locally using the following
# container
#
# docker run --name apt-cacher-ng --init -d --restart=always \
#   --publish 3142:3142 \
#   --volume /mnt/c/apt-cacher-ng:/var/cache/apt-cacher-ng \
#   sameersbn/apt-cacher-ng
#
# Then I change my running distro's apt proxy in this way
#
# (Comment out the following 2 lines if you don't prefer to do it)

RUN echo "Acquire::HTTP::Proxy \"${APT_PROXY}\";" >> /etc/apt/apt.conf.d/01proxy \
 && echo 'Acquire::HTTPS::Proxy "false";' >> /etc/apt/apt.conf.d/01proxy
 
RUN useradd -m ${USER} && echo "${USER}:246826" | chpasswd && usermod -a -G root ${USER}

RUN apt-get update && apt-get -y upgrade && apt-get install -y sudo

RUN adduser ${USER} sudo

RUN apt install -y build-essential git \
    subversion make cmake pkg-config \
    bison yasm meson ninja-build automake \
    autoconf gcc g++ gdb clang lldb \
    python3.10-dev cython3 libx11-dev \
    libxxf86vm-dev libxcursor-dev libxi-dev \
    libxrandr-dev libxinerama-dev libglew-dev \
    wget curl nano libasound2-dev libsoundio-dev\
    portaudio19-dev libpulse-dev wayland-scanner++ \
    help2man libwayland-dev libwayland-bin \
    wayland-protocols libegl-dev libxkbcommon-dev \
    libdbus-1-dev linux-libc-dev

RUN wget https://ftp.gnu.org/gnu/automake/automake-1.15.tar.gz && \
    gunzip ./automake-1.15.tar.gz && \
    tar -xvf ./automake-1.15.tar && \
    cd automake-1.15 && \
    ./configure && \
    make -j12 && \
    make install && cd ..

USER $USER

RUN git config --global user.name $GIT_USER && git config --global user.email $GIT_EMAIL

ENTRYPOINT [ "/bin/bash" ]

