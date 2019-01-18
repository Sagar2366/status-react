GIT_COMMIT = $(shell git rev-parse --short HEAD)
GIT_ROOT = $(shell git rev-parse --show-toplevel)

# WARNING: Remember to change the tag when updating the image
IMAGE_TAG = 1.1.0
IMAGE_NAME = statusteam/status-build-base:$(IMAGE_TAG)

__read__toolversion__ = $(shell grep $(1) ../../../.TOOLVERSIONS | cut -d'=' -f2-)

build:
	docker build \
		--build-arg="LEIN_VERSION=$(call __read__toolversion__, lein)" \
		--build-arg="NODE_VERSION=$(call __read__toolversion__, node)" \
		--build-arg="YARN_VERSION=$(call __read__toolversion__, yarn)" \
		--build-arg="NVM_VERSION=$(call  __read__toolversion__, nvm)" \
		--label="commit=$(GIT_COMMIT)" \
		-t $(IMAGE_NAME) .

push: build
	docker push $(IMAGE_NAME)
