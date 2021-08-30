ARG BASE_IMAGE=ubuntu:21.04
FROM ${BASE_IMAGE} as builder

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -y \
    && dpkg --add-architecture i386 \
    && apt-get update -y \
	&& apt-get install -y \
    git make gcc ccache gcc-multilib flex bison gcc-mingw-w64 pkg-config \
    libx11-dev:i386 libfreetype6-dev:i386 libfreetype-dev:i386 \
    libxcursor-dev:i386 libxi-dev:i386 libxext-dev:i386 \
    libxxf86vm-dev:i386 libxrandr-dev:i386 libxinerama-dev:i386 \
    libxcomposite-dev:i386 libgl1-mesa-glx:i386 libgl1-mesa-dri:i386 \
    libosmesa6-dev:i386 libpcap-dev:i386 libdbus-1-dev:i386 libsane-dev:i386 \
    libusb-1.0-0-dev:i386 libv4l-dev:i386 libgphoto2-dev:i386 \
    liblcms2-dev:i386 libpulse-dev:i386 libgstreamer-plugins-base1.0-dev:i386 \
    oss4-dev libudev-dev:i386 libsdl2-dev:i386 libfaudio-dev:i386 \
    libcapi20-dev:i386 libcups2-dev:i386 libfontconfig-dev:i386 \
    libgsm1-dev:i386 libkrb5-dev:i386 libjxr-dev \
    libmpg123-dev:i386 libopenal-dev:i386 libvulkan-dev:i386 \
    libvkd3d-dev:i386 libldap2-dev:i386 \
    gettext libxml2-dev:i386 libxslt1-dev:i386 libgnutls28-dev:i386 \
    wget

ARG WINE_VERSION="6.16"
RUN git clone --depth 1 --branch wine-${WINE_VERSION} git://source.winehq.org/git/wine.git \
    && cd wine \
    && wget https://raw.githubusercontent.com/castaneai/winepatches/main/0001-winex11.drv-Support-child-window-rendering-for-Vulka.patch \
    && git apply *.patch \
    && CC='ccache gcc' PKG_CONFIG_PATH=/usr/lib/i386-linux-gnu/pkgconfig ./configure \
    && make -j16 \
    && make install

FROM ${BASE_IMAGE}
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
