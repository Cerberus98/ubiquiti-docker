FROM debian:stretch
MAINTAINER Antony Messerli "antony@mes.ser.li"

# Must be passed in at build time with --build-arg
ARG UNIFI_VIDEO_VERSION
ARG UNIFI_VIDEO_DEB_URL

RUN echo "debconf debconf/frontend select Noninteractive" | debconf-set-selections && \
    apt-get update && apt-get install -y vim curl && \
    curl -L ${UNIFI_VIDEO_DEB_URL} -o /tmp/unifi-video.deb && \
    mkdir -p /var/cache/unifi-video && \
    mkdir -p /var/run/unifi-video && \
    dpkg -i /tmp/unifi-video.deb || /bin/true && apt-get -yf --force-yes install && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    sed -i.bak 's/ulimit -H -c 200//g' /usr/sbin/unifi-video && \
    sed -i.bak 's/PKGUSER=unifi-video/PKGUSER=root/g' /usr/sbin/unifi-video && \
    chmod 755 /usr/sbin/unifi-video && \
    chown -R root.root /var/lib/unifi-video /usr/lib/unifi-video /var/log/unifi-video

ADD start.sh /bin
RUN /bin/chmod +x /bin/start.sh

# No reason to expose the MongoDB port 7441 to the outside for most people
EXPOSE 1935 6666 7080 7443 7445 7446 7447 
#EXPOSE 7441

ENV SHELL /bin/bash

CMD []
ENTRYPOINT ["/bin/start.sh"]
