FROM ubuntu:18.04

# install required packages
RUN apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y \
        aptitude iputils-ping net-tools man vim openssh-server python \
    && rm -rf /var/lib/apt/lists/*

# ensure that required directories are created
RUN mkdir /var/run/sshd
RUN mkdir /root/.ssh
