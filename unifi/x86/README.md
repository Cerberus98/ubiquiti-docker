
Docker image antonym/unifi-controller are Docker images to run
the Unifi Controller on a Synology NAS.  The image requires you
to you to run the container with a number of extra capabilities, as
well as disabling apparmor for the container and mounting a host
volume to store all your configuration, videos, and mongodb database
files into the /var/lib/unifi-video location on the container.

The Synology Docker UI doesn't provide the ability to set these
more esoteric security policies, so if you want to start it from there
you will need to select "privileged" mode for the container.

Alternately, if you prefer to have a more reduced security
profile you can run it from the command line.  Here's an example
from my system, you'll probably need to change the directory for
the host volume mount:

```
docker run --restart always --name unifi -h unifi -p 8080:8080 -p 8443:8443 -p 8843:8843 -p 8880:8880 -p 3478:3478/udp -p 10001:10001 -d -v /volume1/docker/unifi:/var/lib/unifi antonym/unifi-controller:latest
```
If you feel like building the container manually:

```
docker build --no-cache --build-arg UNIFI_VERSION=5.4.19 --build-arg UNIFI_DEB_URL=https://dl.ubnt.com/unifi/5.4.19/unifi_sysvinit_all.deb -t antonym/unifi-controller:5.4.19 --rm .
```
