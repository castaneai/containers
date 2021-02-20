FROM ubuntu:20.10

MAINTAINER castaneai <castaneai@castaneai.net>
LABEL org.opencontainers.image.source https://github.com/castaneai/containers

RUN apt update -y \
	&& apt install -y wget software-properties-common gpg-agent \
	&& dpkg --add-architecture i386 \
    && wget -qO - https://dl.winehq.org/wine-builds/winehq.key | apt-key add - \
    && add-apt-repository -y 'deb https://dl.winehq.org/wine-builds/ubuntu/ groovy main' \
    && apt install -y --install-recommends winehq-stable=6.0.\* -o APT::Immediate-Configure=0
