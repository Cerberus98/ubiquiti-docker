FROM arm32v7/debian:stretch-slim
MAINTAINER Matthew Dietz "matthew.dietz@gmail.com"

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

# Stretch-backports has the fixed openjdk8
# Raspbian repo has a functional mongodb-server package
RUN echo "debconf debconf/frontend select Noninteractive" | debconf-set-selections && \
    echo "deb http://http.debian.net/debian stretch-backports main" >> /etc/apt/sources.list.d/stretch-backports.list && \
    apt update && apt-get install -yf gnupg2 curl binutils && \
    echo "deb http://raspbian.raspberrypi.org/raspbian/ stretch main contrib non-free rpi" >> /etc/apt/sources.list && \
    curl -s https://archive.raspbian.org/raspbian.public.key | apt-key add - && \
    apt update && \
    apt install -y cron procps net-tools vim mongodb-server && \
    mkdir -p /usr/share/man/man1/
# Separate this into a distinct step in case it fails so we don't don't have to wait
# on the prior RUN to finish again
RUN curl -L ${UNIFI_DEB_URL} -o /tmp/unifi.deb
RUN apt-get install -yf -t stretch-backports ca-certificates-java jsvc libcap2 default-jre && \
    dpkg -i /tmp/unifi.deb || /bin/true && apt -yf install && \
    /etc/init.d/unifi start && /etc/init.d/unifi stop && \
    apt clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /var/lib/unifi/* /usr/lib/unifi/{data.,logs.,run.}*

# TODO Also I think I should replace the unifi init so I can send the output
# and error logs to an absolute path. YOu can set to /path/to/file
# https://linux.die.net/man/1/jsvc

# No reason to expose the MongoDB port 27117 to the outside for most people
EXPOSE 3478/udp 8080 8443 8843 8880 10001/udp

#EXPOSE 27117

ENV SHELL /bin/bash

CMD []
ENTRYPOINT ["/bin/start.sh"]
