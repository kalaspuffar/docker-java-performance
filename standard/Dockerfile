FROM debian:stable-slim

ADD jdk-10.0.1_linux-x64_bin.tar.gz /opt
ADD apache-maven-3.5.3-bin.tar.gz /opt

ENV PATH=$PATH:/opt/jdk-10.0.1/bin:/opt/apache-maven-3.5.3/bin
ENV JAVA_HOME=/opt/jdk-10.0.1

ADD simplewebserver /opt/simplewebserver
WORKDIR /opt/simplewebserver
RUN mvn package

CMD java -jar target/simplewebserver-1.0-SNAPSHOT-jar-with-dependencies.jar