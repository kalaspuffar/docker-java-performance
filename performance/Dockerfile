FROM debian:stable-slim as build
ADD jdk-10.0.1_linux-x64_bin.tar.gz /opt
ADD apache-maven-3.5.3-bin.tar.gz /opt

ENV PATH=$PATH:/opt/jdk-10.0.1/bin:/opt/apache-maven-3.5.3/bin
ENV JAVA_HOME=/opt/jdk-10.0.1

ADD simplewebserver /opt/simplewebserver
WORKDIR /opt/simplewebserver

RUN ls /opt/jdk-10.0.1/bin
RUN jlink --compress=2 --module-path /opt/jdk-10.0.1/jmods --add-modules java.base,jdk.httpserver --output /linked

RUN mvn package

RUN /linked/bin/java -XX:DumpLoadedClassList=simplewebserver.classlist -jar target/simplewebserver-1.0-SNAPSHOT-jar-with-dependencies.jar debug
RUN /linked/bin/java -Xshare:dump -XX:+UnlockDiagnosticVMOptions -XX:SharedClassListFile=simplewebserver.classlist -XX:SharedArchiveFile=simplewebserver.jsa

RUN apt-get update && apt-get install -y paxctl
RUN paxctl -c /linked/bin/java
RUN paxctl -m /linked/bin/java

FROM alpine:latest
COPY --from=build /linked /linked
COPY --from=build /opt/simplewebserver/target/simplewebserver-1.0-SNAPSHOT-jar-with-dependencies.jar /opt
COPY --from=build /opt/simplewebserver/simplewebserver.jsa /opt

RUN echo 'export JAVA_HOME=/linked' > /etc/profile.d/java.sh
RUN echo 'export PATH=$PATH:$JAVA_HOME/bin' > /etc/profile.d/java.sh

EXPOSE 8080

CMD /linked/bin/java -XX:+UnlockCommercialFeatures -XX:+UseAppCDS -Xshare:on -XX:SharedArchiveFile=/opt/simplewebserver.jsa -jar /opt/simplewebserver-1.0-SNAPSHOT-jar-with-dependencies.jar