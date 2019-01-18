__toolversion = $(shell $(GIT_ROOT)/scripts/toolversion $(1))

GIT_COMMIT = $(shell git rev-parse --short HEAD)
GIT_ROOT = $(shell git rev-parse --show-toplevel)

RNATIVE_VERSION  = $(call __toolversion, react_native_desktop)
QT_CI_COMMIT = $(call __toolversion, qtci)
QT_VERSION = $(call __toolversion, qt)
QT_MD5SUM  = $(call __toolversion, qt_md5)
QT_ARCHIVE = qt-opensource-linux-x64-$(QT_VERSION).run
QT_URL = https://download.qt.io/archive/qt

# WARNING: Remember to change the tag when updating the image
IMAGE_TAG = 1.0.0
IMAGE_NAME = statusteam/status-build-linux:$(IMAGE_TAG)

build: $(QT_ARCHIVE)
	docker build \
		--build-arg="QT_VERSION=$(QT_VERSION)" \
		--build-arg="QT_CI_COMMIT=$(QT_CI_COMMIT)" \
		--build-arg="RNATIVE_VERSION=$(RNATIVE_VERSION)" \
		--label="commit=$(GIT_COMMIT)" \
		-t $(IMAGE_NAME) .; \

$(QT_ARCHIVE):
	wget $(QT_URL)/$(call __major_version__, $(QT_VERSION))/$(QT_VERSION)/$(QT_ARCHIVE)
	echo "$(QT_MD5SUM)  $(QT_ARCHIVE)" | md5sum --check

push: build
	docker push $(IMAGE_NAME)
