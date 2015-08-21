FROM java:7

ENV TOMCAT_FAMILY 7
ENV TOMCAT_VERSION 7.0.62
ENV CATALINA_HOME /opt/tomcat
ENV PATH $PATH:$CATALINA_HOME/bin

# update packages and install maven
RUN  \
  export DEBIAN_FRONTEND=noninteractive && \
  sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y vim wget curl git maven wget software-properties-common

# Get Tomcat
RUN wget --quiet --no-cookies http://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_FAMILY}/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz -O /tmp/tomcat.tgz

# Uncompress
RUN tar xzvf /tmp/tomcat.tgz -C /opt
RUN mv /opt/apache-tomcat-${TOMCAT_VERSION} /opt/tomcat
RUN rm /tmp/tomcat.tgz

# Remove garbage
RUN rm -rf /opt/tomcat/webapps/examples
RUN rm -rf /opt/tomcat/webapps/docs
RUN rm -rf /opt/tomcat/webapps/ROOT

# package webapp
ONBUILD COPY . /usr/src/app
ONBUILD WORKDIR /usr/src/app
ONBUILD RUN mvn package
ONBUILD RUN cp target/ROOT.war /opt/tomcat/webapps/

# Launch Tomcat
ONBUILD EXPOSE 8080
ONBUILD EXPOSE 8009
ONBUILD WORKDIR /opt/tomcat

ONBUILD CMD ["/opt/tomcat/bin/catalina.sh", "run"]