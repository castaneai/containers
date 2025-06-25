BASE_IMAGE := ubuntu:24.04
WINE_VERSION := 10.10

build:
	docker build \
	--build-arg BASE_IMAGE=${BASE_IMAGE} \
	--build-arg WINE_VERSION=${WINE_VERSION} \
	-t ghcr.io/castaneai/wine-wow64:${WINE_VERSION} .
