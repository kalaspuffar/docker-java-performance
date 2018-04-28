# docker-java-performance
An simple example of performance enhancements for java services

In order to use this repository you clone it using the command below. Notice that you need recursive in order to get the webserver used in this example.
```
git clone --recursive https://github.com/kalaspuffar/docker-java-performance.git
```

Requirements
* Apache Maven
* OracleJDK 10
* OpenJDK 11 for Alpine / Musl

Sizes (Your miles may vary)
* Standard 664MB
* Small 38.2MB
* Performance 52.2MB


Below you have three commands to build the different flavors of this example. Standard is a full build with debian and a full java. Small is built on alpine and compressed to be as small as possible. Lastly the performance example is using alpine but tries to cache java classes so the jvm don't need to jit these during start up.
```
docker build -t standard -f standard/Dockerfile .
docker build -t small -f small/Dockerfile .
docker build -t performance -f performance/Dockerfile .
```

Below you have simple commands to start each image interactivly so you can try them out.
```
docker run -p 8080:8080 -i -t standard
docker run -p 8080:8080 -i -t small
docker run -p 8080:8080 -i -t performance
```

Lastly I add this little command so you can see which image you have in our setup and clean up with prune when your done. Notice that that command will remove all images in your environment so DO NOT RUN in production.
```
docker images
docker system prune -a
```