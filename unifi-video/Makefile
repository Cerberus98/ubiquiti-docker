NAME = antonym/unifi-video-controller
VERSION_A = 3.8.0beta1
VERSION_B = 3.8.0-beta1
UNIFI_VIDEO_DEB_URL=https://dl.ubnt.com/firmwares/unifi-video/$(VERSION_A)/unifi-video_$(VERSION_B)-Ubuntu16.04_amd64.deb
.PHONY: all build test tag_latest release

all: build

build:
	docker build --no-cache --build-arg UNIFI_VIDEO_VERSION=$(VERSION) --build-arg UNIFI_VIDEO_DEB_URL=$(UNIFI_VIDEO_DEB_URL) -t $(NAME):$(VERSION) --rm .

test:
	env NAME=$(NAME) VERSION=$(VERSION) ./mocktest.sh

tag_latest:
	docker tag $(NAME):$(VERSION) $(NAME):latest

release: tag_latest
	@if ! docker images $(NAME) | awk '{ print $$2 }' | grep -q -F $(VERSION); then echo "$(NAME) version $(VERSION) is not yet built. Please run 'make build'"; false; fi
	docker push $(NAME)
	@echo "*** Don't forget to create a tag. git tag rel-$(VERSION) && git push origin rel-$(VERSION)"
