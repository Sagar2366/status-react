GIT_COMMIT = $(shell git rev-parse --short HEAD)
GIT_ROOT = $(shell git rev-parse --show-toplevel)

# WARNING: Remember to change the tag when updating the image
IMAGE_TAG = 1.1.0
IMAGE_NAME = statusteam/status-build-base:$(IMAGE_TAG)

__read__toolversion__ = $(shell grep $(1) $(GIT_ROOT)/.TOOLVERSIONS | cut -d'=' -f2-)

build: nvm_install.sh
	docker build \
		--build-arg="LEIN_VERSION=$(call __read__toolversion__, leiningen)" \
		--build-arg="NODE_VERSION=$(call __read__toolversion__, node)" \
		--build-arg="YARN_VERSION=$(call __read__toolversion__, yarn)" \
		--build-arg="NVM_VERSION=$(call  __read__toolversion__, nvm)" \
		--label="commit=$(GIT_COMMIT)" \
		-t $(IMAGE_NAME) .

nvm_install.sh:
	cp $(GIT_ROOT)/scripts/3rd-party/nvm/$(call  __read__toolversion__, nvm)/install.sh nvm_install.sh

push: build
	docker push $(IMAGE_NAME)
