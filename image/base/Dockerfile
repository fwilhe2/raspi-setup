ARG MAVEN_VERSION=3.6.3

FROM debian:buster-slim as builder

ARG MAVEN_VERSION

ADD https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.7%2B10/OpenJDK11U-jdk_aarch64_linux_hotspot_11.0.7_10.tar.gz /jdk.tgz
RUN tar xzf jdk.tgz -C /opt
ADD http://apache.lauf-forum.at/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz /maven.tgz
RUN tar xzf /maven.tgz -C /opt

FROM debian:buster-slim

ARG MAVEN_VERSION

RUN apt-get -y update \
 && apt-get -y dist-upgrade \
 && apt-get -y --no-install-recommends install wget curl ca-certificates unzip \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* \
 && mkdir -p /home/user && echo "user:x:1000:1000:user:/home/user:/bin/bash" >> /etc/passwd \
 && chown -R user /home/user \
 && echo "PATH=/opt/apache-maven-$MAVEN_VERSION/bin:/opt/jdk-11.0.7+10/bin:$PATH" >> /home/user/.bashrc

USER user

COPY --from=builder /opt /opt
