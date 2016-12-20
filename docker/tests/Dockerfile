# Version: 0.1
FROM ubuntu:14.04
MAINTAINER Viktor Farcic "viktor@farcic.com"

#lets configure image to pass through GTO proxy
RUN echo 'Acquire::http::Proxy "http://proxy-sg-singapore.gemalto.com:8080";' >> /etc/apt/apt.conf
RUN echo 'Acquire::https::Proxy "http://proxy-sg-singapore.gemalto.com:8080";' >> /etc/apt/apt.conf
 
RUN export HTTP_PROXY=http://proxy-sg-singapore.gemalto.com:8080
RUN export HTTPS_PROXY=http://proxy-sg-singapore.gemalto.com:8080

# Packages
RUN echo "deb http://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list
RUN apt-get update && \
    apt-get -y --force-yes install --no-install-recommends openjdk-7-jdk mongodb wget sbt && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
RUN wget -q http://downloads.typesafe.com/scala/2.11.5/scala-2.11.5.deb && \
	-e use_proxy=yes -e http_proxy=http://proxy-sg-singapore.gemalto.com:8080 && \
    dpkg -i scala-2.11.5.deb && \
    rm scala-2.11.5.deb

RUN mkdir -p /data/db
ENV TEST_TYPE "spec"
ENV DOMAIN "http://172.17.42.1"
VOLUME ["/source", "/root/.ivy2/cache"]
WORKDIR /source
CMD ["/source/run_tests.sh"]