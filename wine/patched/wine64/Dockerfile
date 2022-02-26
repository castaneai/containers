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
RUN mkdir -p /wine64-build \
    && cd /wine64-build \
    && /wine-source/configure --enable-win64 \
    && make -j16
RUN mkdir -p /wine32-build \
    && cd /wine32-build \
    && PKG_CONFIG_PATH=/usr/lib/i386-linux-gnu/pkgconfig /wine-source/configure --with-wine64=/wine64-build \
    && make -j16 
# > If you do choose to install your WoW64 build, you should run make install in the 32-bit build tree first, then in the 64-bit one. 
# https://wiki.winehq.org/Building_Wine#Shared_WoW64
RUN cd /wine32-build \
    && make install \
    && cd /wine64-build \
    && make install

FROM ${BASE_IMAGE}
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update -y \
    && dpkg --add-architecture i386 \
    && apt-get update -y \
	&& apt-get install -y \
    libasound2:* libasound2-plugins:* libc6:* libfaudio0:* libglib2.0-0:* libgphoto2-6:* libgphoto2-port12:* libgstreamer-plugins-base1.0-0:* libgstreamer1.0-0:* \
    libcap2-bin libcapi20-3:* libcups2:* libdbus-1-3:* \
    liblcms2-2:* libldap-2.4-2:* libmpg123-0:* \
    libpcap0.8:* libpulse0:* libudev1:* \
    libusb-1.0-0:* libx11-6:* libxml2:* libasound2-plugins:* libncurses6:* \
    libfontconfig1:* libfreetype6:* libglu1-mesa:* libgnutls30:* libgsm1:* \
    libjpeg8:* libosmesa6:* libpng16-16:* libsdl2-2.0-0:* libtiff5:* libv4l-0:* \
    libxcomposite1:* libxcursor1:* libxfixes3:* libxi6:* libxinerama1:* libxrandr2:* libxrender1:* libxslt1.1:* libxxf86vm1:* \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /usr/local/bin /usr/local/bin
COPY --from=builder /usr/local/include/wine /usr/local/include/wine
COPY --from=builder /usr/local/lib/wine /usr/local/lib/wine
COPY --from=builder /usr/local/share/wine /usr/local/share/wine
