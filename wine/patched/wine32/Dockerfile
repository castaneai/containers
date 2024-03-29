# https://github.com/moby/moby/issues/38379
ARG BASE_IMAGE

FROM ghcr.io/castaneai/winebuilder:latest as builder
ARG WINE_VERSION
RUN apt-get update -y \
    && apt-get install -y git wget
RUN git clone --depth 1 --branch wine-${WINE_VERSION} https://github.com/wine-mirror/wine /wine-source \
    && cd /wine-source \
    && wget https://raw.githubusercontent.com/castaneai/winepatches/main/0001-winex11.drv-Support-child-window-rendering-for-Vulka.patch \
    && git apply *.patch
RUN mkdir -p /wine32-build \
    && cd /wine32-build \
    && PKG_CONFIG_PATH=/usr/lib/i386-linux-gnu/pkgconfig /wine-source/configure \
    && make -j16 \
    && make install

FROM ${BASE_IMAGE}
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update -y \
    && dpkg --add-architecture i386 \
    && apt-get update -y \
	&& apt-get install -y \
    libasound2:i386 libasound2-plugins:i386 libc6:i386 libfaudio0:i386 libglib2.0-0:i386 libgphoto2-6:i386 libgphoto2-port12:i386 libgstreamer-plugins-base1.0-0:i386 libgstreamer1.0-0:i386 \
    libcap2-bin:i386 libcapi20-3:i386 libcups2:i386 libdbus-1-3:i386 \
    liblcms2-2:i386 libldap-2.4-2:i386 libmpg123-0:i386 \
    libopenal1:i386 libpcap0.8:i386 libpulse0:i386 libudev1:i386 \
    libusb-1.0-0:i386 libx11-6:i386 libxml2:i386 libopencl1:i386 libasound2-plugins:i386 libncurses6:i386 \
    libfontconfig1:i386 libfreetype6:i386 libglu1-mesa:i386 libgnutls30:i386 libgsm1:i386 \
    libjpeg8:i386 libosmesa6:i386 libpng16-16:i386 libsdl2-2.0-0:i386 libtiff5:i386 libv4l-0:i386 \
    libxcomposite1:i386 libxcursor1:i386 libxfixes3:i386 libxi6:i386 libxinerama1:i386 libxrandr2:i386 libxrender1:i386 libxslt1.1:i386 libxxf86vm1:i386 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /usr/local/bin /usr/local/bin
COPY --from=builder /usr/local/include/wine /usr/local/include/wine
COPY --from=builder /usr/local/lib/wine /usr/local/lib/wine
COPY --from=builder /usr/local/share/wine /usr/local/share/wine
