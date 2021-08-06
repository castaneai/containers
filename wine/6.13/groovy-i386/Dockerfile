FROM ubuntu:20.10

LABEL org.opencontainers.image.source https://github.com/castaneai/containers

RUN apt update -y \
	&& apt install -y wget software-properties-common gpg-agent \
	&& dpkg --add-architecture i386 \
    && wget -qO - https://dl.winehq.org/wine-builds/winehq.key | apt-key add - \
    && add-apt-repository -y 'deb https://dl.winehq.org/wine-builds/ubuntu/ groovy main' \
    && apt install -y --install-recommends winehq-devel=6.13\* wine-devel-amd64- libgl1-mesa-dri:i386- -o APT::Immediate-Configure=0 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
