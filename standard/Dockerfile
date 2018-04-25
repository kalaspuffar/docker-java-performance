FROM debian:stable-slim

ADD "../jdk-10.0.1_linux-x64_bin.tar.gz" /opt/jdk
ENV PATH=$PATH:/opt/jdk/bin

ADD "../simplewebserver" /opt/simplewebserver
WORKDIR /opt/simplewebserver
CMD "mvn package"

RUN "java -jar target/simplewebserver-1.0-SNAPSHOT-jar-with-dependencies.jar"