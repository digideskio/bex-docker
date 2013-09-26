# Jira 6.1 (EAP)

# VERSION       0.1

# Use the Ubuntu base image provided by dotCloud
FROM ubuntu
MAINTAINER Victor Hooi <victor@bex.com.au>
# Make sure the package repository is up to date
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y

# Fake a Fuse install - openjdk-7-jdk seems to pull in fuse, but doens't actively use it
# https://gist.github.com/henrik-muehe/6155333
RUN apt-get install libfuse2
RUN cd /tmp ; apt-get download fuse
RUN cd /tmp ; dpkg-deb -x fuse_* .
RUN cd /tmp ; dpkg-deb -e fuse_*
RUN cd /tmp ; rm fuse_*.deb
RUN cd /tmp ; echo -en '#!/bin/bash\nexit 0\n' > DEBIAN/postinst
RUN cd /tmp ; dpkg-deb -b . /fuse.deb
RUN cd /tmp ; dpkg -i /fuse.deb

# Install OpenJDK7
RUN apt-get install -y openjdk-7-jdk

# Download the JIRA installer
# NB: ADD commands can't be cached, nor can anything after an ADD - https://github.com/dotcloud/docker/issues/1903
#ADD http://www.atlassian.com/software/jira/downloads/binary/atlassian-jira-6.1-rc2-x64.bin /
ADD ./atlassian-jira-6.1-rc2-x64.bin /

# Run the installer
ADD response.varfile /
RUN chmod a+x /atlassian-jira-6.1-rc2-x64.bin
RUN /atlassian-jira-6.1-rc2-x64.bin -q -varfile response.varfile

#
EXPOSE 80:8080

CMD ["/opt/atlassian/jira/bin/start-jira.sh", "-fg"]
