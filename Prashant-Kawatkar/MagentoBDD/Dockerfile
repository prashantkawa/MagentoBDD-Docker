FROM ubuntu:latest

MAINTAINER Prashant Kawatkar (https://github.com/prashantkawa)

# update dpkg repositories
RUN apt-get update

# install wget
RUN apt-get install -y wget
RUN apt-get install sudo
RUN apt-get -y install curl
RUN apt-get install -y p7zip \
    p7zip-full \
    unace \
    zip \
    unzip \
    bzip2

ARG MAVEN_VERSION=3.6.3
ARG USER_HOME_DIR="/root"
ARG BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries

RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
 && curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
 && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
 && rm -f /tmp/apache-maven.tar.gz \
 && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven
RUN export MAVEN_HOME
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

# remove download archive files
RUN apt-get clean

RUN apt-get update && \
    apt-get install -y openjdk-17-jdk && \
    apt-get install -y ant && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/ && \
    rm -rf /var/cache/oracle-jdk17-installer;

ENV JAVA_HOME /usr/lib/jvm/java-17-openjdk-amd64/
RUN export JAVA_HOME
RUN export PATH=$JAVA_HOME/bin:$PATH

# Update aptitude with new repo
RUN apt-get update \
 && apt-get install -y git

RUN git clone https://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@github.com/prashantkawa/dockerise3.git

WORKDIR /dockerise3/Prashant-Kawatkar/hellocucumber
RUN mvn dependency:copy-dependencies

#Version numbers
ARG CHROME_VERSION=83.0.4103.116
ARG CHROMDRIVER_VERSION=83.0.4103.39

#Install Chrome
RUN mkdir -p /app/bin
RUN curl https://chromedriver.storage.googleapis.com/$CHROMDRIVER_VERSION/chromedriver_linux64.zip -o /tmp/chromedriver.zip \
    && unzip /tmp/chromedriver.zip -d /app/bin/ \
    && rm /tmp/chromedriver.zip
RUN chmod +x /app/bin/chromedriver


RUN apt update -y && sudo apt upgrade -y