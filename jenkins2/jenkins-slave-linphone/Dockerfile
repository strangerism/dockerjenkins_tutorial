FROM openjdk:8-jdk

MAINTAINER Massimo Virgilio

ARG ANDROID_COMPILE_SDK="25"
ARG ANDROID_BUILD_TOOLS="24.0.0"
ARG ANDROID_SDK_TOOLS="24.4.1"

RUN apt-get update && apt-get install -y \
 yasm \
 nasm \
 ant \
 python \
 cmake \
 vim-common \
 patch \
 pkg-config \
 git \
 curl \
 unzip \
 libqt5widgets5 \
 aptitude

RUN dpkg --add-architecture i386

RUN aptitude update && aptitude install -y \
 libstdc++6:i386 \
 libgcc1:i386 \
 zlib1g:i386 \
 libncurses5:i386


# Create Jenkins User
RUN useradd jenkins -m -s /bin/bash

RUN chown -R jenkins /home/jenkins
RUN chgrp -R jenkins /home/jenkins

# Add the jenkins user to sudoers
#RUN echo "jenkins    ALL=(ALL)    ALL" >> etc/sudoers

USER jenkins

# Expose user HOME to docker build environment
ENV HOME /home/jenkins

WORKDIR $HOME

# Add Sndroid SDK
ENV ANDROID_HOME $HOME/android-sdk-linux

 # This is the place where will sit the .android folder commonly used
 # by `android create avd` and `emulator64-x86 -avd test`
 # Without this the two commande uses different folder and produce an error of mismatch
 ENV ANDROID_SDK_HOME $ANDROID_HOME 

RUN curl https://dl.google.com/android/android-sdk_r${ANDROID_SDK_TOOLS}-linux.tgz | tar xz \
 && echo y  | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter android-${ANDROID_COMPILE_SDK} \
 && echo y  | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter platform-tools \
 && echo y  | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter build-tools-${ANDROID_BUILD_TOOLS} \
 && echo y  | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter extra-android-m2repository \
 && echo y  | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter extra-google-google_play_services \
 && echo y  | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter extra-google-m2repository \
 && echo y  | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter sys-img-x86-google_apis-${ANDROID_COMPILE_SDK} \
 && echo no | $ANDROID_HOME/tools/android create avd -n test -t android-${ANDROID_COMPILE_SDK} --abi google_apis/x86
ENV PATH $PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

# adding gradle dependencies
RUN echo y | android update sdk -u -a -t build-tools-25.0.2 && \
	echo y | android update sdk -u -a -t android-23

# Add android-wait-for-emulator executable
RUN mkdir -p bin \
 && cd bin \
 && curl https://raw.githubusercontent.com/travis-ci/travis-cookbooks/0f497eb71291b52a703143c5cd63a217c8766dc9/community-cookbooks/android-sdk/files/default/android-wait-for-emulator > android-wait-for-emulator \
 && chmod +x android-wait-for-emulator
ENV PATH $PATH:$HOME/bin

# Add Android NDK
RUN curl https://dl.google.com/android/repository/android-ndk-r12b-linux-x86_64.zip > android-ndk-r12b.zip \
 && unzip android-ndk-r12b.zip \
 && rm android-ndk-r12b.zip
ENV ANDROID_NDK_HOME $HOME/android-ndk-r12b
ENV PATH $PATH:$ANDROID_NDK_HOME

# USER root
# # install cmake 3.9.4
# RUN apt-get purge -y cmake && \
# 	mkdir ~/temp && \
# 	cd ~/temp && \
# 	wget https://cmake.org/files/v3.9/cmake-3.9.4.tar.gz && \
# 	tar -xzvf cmake-3.9.4.tar.gz && \
# 	cd cmake-3.9.4/
# RUN	cd ~/temp/cmake-3.9.4/ && \
# 	./bootstrap
# RUN	cd ~/temp/cmake-3.9.4/ && \
# 	make -j4
# RUN	cd ~/temp/cmake-3.9.4/ && \
# 	make install
# RUN rm -R temp

# USER jenkins

# Set Name Servers
COPY /files/resolv.conf /etc/resolv.conf

# Install linphone

# RUN git clone https://github.com/BelledonneCommunications/linphone-android.git
# RUN cd linphone-android && \
# 	git checkout 3.2.7 && \
# 	git submodule sync && git submodule update --init --recursive

# RUN cd linphone-android && ./prepare.py 

# RUN cd linphone-android && make 
#
