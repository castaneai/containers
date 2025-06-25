# https://github.com/moby/moby/issues/38379
ARG BASE_IMAGE

FROM ghcr.io/castaneai/winebuilder:wow64 as builder
ARG WINE_VERSION
RUN apt-get update -y \
    && apt-get install -y git wget
RUN git clone --depth 1 --branch wine-${WINE_VERSION} https://gitlab.winehq.org/wine/wine.git /wine-source \
    && cd /wine-source \
RUN mkdir -p /wine-build \
    && cd /wine-build \
    && /wine-source/configure --with-gstreamer --enable-archs=x86_64,i386 \
    && make -j16 \
    && make install

FROM ${BASE_IMAGE}
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update -y \
    && apt-get update -y \
	&& apt-get install -y \
    libasound2 libasound2-plugins libc6 libfaudio0 libglib2.0-0 libgphoto2-6 libgphoto2-port12 libgstreamer-plugins-base1.0-0 libgstreamer1.0-0 \
    libcap2-bin libcapi20-3 libcups2 libdbus-1-3 \
    liblcms2-2 libldap-2.4-2 libmpg123-0 \
    libopenal1 libpcap0.8 libpulse0 libudev1 \
    libusb-1.0-0 libx11-6 libxml2 libopencl1 libasound2-plugins libncurses6 \
    libfontconfig1 libfreetype6 libglu1-mesa libgnutls30 libgsm1 \
    libjpeg8 libosmesa6 libpng16-16 libsdl2-2.0-0 libtiff5 libv4l-0 \
    libxcomposite1 libxcursor1 libxfixes3 libxi6 libxrandr2 libxrender1 libxslt1.1 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /usr/local/bin /usr/local/bin
COPY --from=builder /usr/local/include/wine /usr/local/include/wine
COPY --from=builder /usr/local/lib/wine /usr/local/lib/wine
COPY --from=builder /usr/local/share/wine /usr/local/share/wine
