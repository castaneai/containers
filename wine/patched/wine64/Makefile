BASE_IMAGE := ubuntu:20.04
WINE_VERSION := 6.16

build:
	docker build \
	--build-arg BASE_IMAGE=${BASE_IMAGE} \
	--build-arg WINE_VERSION=${WINE_VERSION} \
	-t ghcr.io/castaneai/wine64:${WINE_VERSION} .
