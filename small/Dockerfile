FROM alpine:latest as build
ADD openjdk-11-ea+9_linux-x64-musl_bin.tar.gz /opt
ADD apache-maven-3.5.3-bin.tar.gz /opt

ENV JAVA_HOME=/opt/jdk-11
ENV PATH=$PATH:$JAVA_HOME/bin:/opt/apache-maven-3.5.3/bin

ADD simplewebserver /opt/simplewebserver
WORKDIR /opt/simplewebserver

RUN jlink --compress=2 --module-path $JAVA_HOME/jmods --add-modules java.base,jdk.httpserver --output /linked

RUN mvn package

FROM alpine:latest
COPY --from=build /linked /linked
COPY --from=build /opt/simplewebserver/target/simplewebserver-1.0-SNAPSHOT-jar-with-dependencies.jar /opt/simplewebserver-1.0-SNAPSHOT-jar-with-dependencies.jar

RUN echo 'export JAVA_HOME=/linked' > /etc/profile.d/java.sh
RUN echo 'export PATH=$PATH:$JAVA_HOME/bin' > /etc/profile.d/java.sh

EXPOSE 8080

CMD /linked/bin/java -jar /opt/simplewebserver-1.0-SNAPSHOT-jar-with-dependencies.jar