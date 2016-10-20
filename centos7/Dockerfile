FROM centos:7.2.1511

WORKDIR /workdir

# Instal dependencies
RUN yum install -y epel-release && \
    yum install -y java-1.8.0-openjdk-devel.x86_64 \
      git which file make unzip eventlog eventlog-devel eventlog-static \
      libcurl-devel libnet-devel libwrap-devel pcre-devel \
      autoconf-archive automake libtool bison flex glib2-devel openssl-devel

# Add gradle
ARG GRADLE_VERSION=3.1
RUN curl -L -o gradle.zip https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-all.zip && \
    unzip gradle.zip && \
    rm gradle.zip && \
    ln -s /workdir/gradle-${GRADLE_VERSION}/bin/gradle /usr/bin/gradle
RUN echo "export JAVA_HOME=/etc/alternatives/java_sdk/" >>~/.bashrc

# Clone syslog-ng repo
RUN git clone https://github.com/balabit/syslog-ng
RUN cd syslog-ng && git submodule update --init --recursive

# Compile
RUN mkdir dist && \
    cd syslog-ng && \
    ./autogen.sh
RUN cd syslog-ng && \
    ./configure --prefix=/workdir/dist/syslog-nf-RHEL7-ose-3.8.1 \
      --disable-mongodb --disable-legacy-mongodb-options --disable-smtp --disable-http --disable-sql \
      --disable-amqp --disable-stom --enable-java
RUN cd syslog-ng && \
    make && \
    make install