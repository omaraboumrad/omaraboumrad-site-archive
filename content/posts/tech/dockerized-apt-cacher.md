Title: Speed up installs with dockerized apt cacher
Slug: speed-up-installs-with-dockerized-apt-cacher
Date: 2015-04-23 15:06:18
Tags: tips, docker
Summary: Speeding up install with a dockerized apt cacher
Status: Published
Schema: Article

Everyone hates downloading the same package multiple times. It gets even worse when you're doing it constantly when building docker images.

The process of dockerizing **apt-cacher-ng** is very well documented [here](https://docs.docker.com/examples/apt-cacher-ng/). Refer to the link for an in depth read, or continue for a quick sample.

Follows is an ascii cast of the process. Transcript will follow.

<script type="text/javascript" src="https://asciinema.org/a/19118.js" id="asciicast-19118" async></script>

We will need to build the docker image first. You can use this *Dockerfile*

    # Taken from: https://docs.docker.com/examples/apt-cacher-ng/
    FROM        ubuntu
    MAINTAINER  SvenDowideit@docker.com

    VOLUME  /var/cache/apt-cacher-ng
    RUN     apt-get update && apt-get install -y apt-cacher-ng

    EXPOSE  3142
    CMD     chmod 777 /var/cache/apt-cacher-ng && \
            /etc/init.d/apt-cacher-ng start && \
            tail -f /var/log/apt-cacher-ng/*

Build it using

    $ docker build -t cacher:latest .

When the cacher image is done building, run it using the following:

    $ docker run -d -p 3142:3142 --name cacher cacher:latest

Now let's fire (#1) a *ubuntu:latest* container and make it communicate with our cacher and install a package.

    $ docker run --link cacher:cacher -e http_proxy=http://cacher:3142 -ti ubuntu:latest apt-get install -y python 

The first install of a specific package will download it from the internet and cache it. Here's an example excerpt of the log from the cacher:

    $ docker logs cacher      

    1429789936|I|1190396|172.17.0.106|uburep/pool/main/p/python2.7/python2.7-minimal_2.7.6-8_amd64.deb         1429789936|O|1190427|172.17.0.106|uburep/pool/main/p/python2.7/python2.7-minimal_2.7.6-8_amd64.deb


Now re-run another container using the same command issued before (ref #1). Notice how instead of downloading from the internet, *apt-get* retrieves the package instantly from the cacher.
