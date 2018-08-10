FROM debian:stretch
MAINTAINER Antony Messerli "antony@mes.ser.li"

# Must be passed in at build time with --build-arg
ARG UNIFI_VERSION
ARG UNIFI_DEB_URL

ADD start.sh /bin
RUN /bin/chmod +x /bin/start.sh

# We download the exact version of Unifi that we want so that we know our 
#  docker tags are correct instead of apt-get update && apt-get -y install unifi
#
# We need to run the unifi script once because it sets up all the 
#  /var/lib/unifi and /usr/lib/unifi symlinks for us.  But we don't
#  want to keep any of that data around, we want our union mount to
#  replace /var/lib/unifi so we remove a bunch of dirs at the end
RUN echo "debconf debconf/frontend select Noninteractive" | debconf-set-selections && \
    apt update && \
    apt install -y curl cron procps net-tools vim && \
    curl -L ${UNIFI_DEB_URL} -o /tmp/unifi.deb && \
    dpkg -i /tmp/unifi.deb || /bin/true && apt -yf --force-yes install && \
    /etc/init.d/unifi start && /etc/init.d/unifi stop && \ 
    apt clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /var/lib/unifi/* /usr/lib/unifi/{data.,logs.,run.}* 

# No reason to expose the MongoDB port 27117 to the outside for most people
EXPOSE 3478/udp 8080 8443 8843 8880 10001
#EXPOSE 27117

ENV SHELL /bin/bash

CMD []
ENTRYPOINT ["/bin/start.sh"]
