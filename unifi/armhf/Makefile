NAME = cerberus/unifi-controller
VERSION = 5.6.37
UNIFI_DEB_URL=https://www.ubnt.com/downloads/unifi/$(VERSION)/unifi_sysvinit_all.deb

.PHONY: all build test tag_latest release

all: build

build:
	docker build --build-arg UNIFI_VERSION=$(VERSION) --build-arg UNIFI_DEB_URL=$(UNIFI_DEB_URL) -t $(NAME):$(VERSION) .

run:
	docker run --privileged --restart always --name unifi -h unifi -p 8080:8080 -p 8443:8443 -p 8843:8843 -p 8880:8880 -p 3478:3478/udp -p 10001:10001/udp -d -v /var/lib/unifi:/var/lib/unifi $(NAME):$(VERSION)
