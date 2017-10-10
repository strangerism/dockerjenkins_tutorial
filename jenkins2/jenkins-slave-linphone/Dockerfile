FROM dbndev/linphone-android:latest
MAINTAINER Massimo Virgilio

# Create Jenkins User
RUN useradd jenkins -m -s /bin/bash

RUN chown -R jenkins /home/jenkins
RUN chgrp -R jenkins /home/jenkins
RUN chown -R jenkins /opt

# Add the jenkins user to sudoers
#RUN echo "jenkins    ALL=(ALL)    ALL" >> etc/sudoers

# Set Name Servers
COPY /files/resolv.conf /etc/resolv.conf



