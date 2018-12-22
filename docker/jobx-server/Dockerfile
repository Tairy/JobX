FROM java:8-jdk

LABEL maintainer=tairyguo@gmail.com

WORKDIR /working

ADD pom.xml /working/pom.xml
ADD jobx-agent /working/jobx-agent
ADD jobx-api /working/jobx-api
ADD jobx-common /working/jobx-common
ADD jobx-registry /working/jobx-registry
ADD jobx-rpc /working/jobx-rpc
ADD jobx-server /working/jobx-server
ADD jobx-examples /working/jobx-examples
ADD settings.xml /working/settings.xml
ADD checkstyle.xml /checkstyle.xml
ADD deployment /working/deployment

RUN cd /working \
    && wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz \
    && tar -zxvf apache-maven-3.5.4-bin.tar.gz \
    && /working/apache-maven-3.5.4/bin/mvn -f pom.xml clean install -Dmaven.test.skip=true --settings /working/settings.xml \
    && chmod +x /working/deployment/server.sh

EXPOSE 1577

CMD /working/deployment/server.sh



