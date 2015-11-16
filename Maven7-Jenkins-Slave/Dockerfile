FROM debian:jessie

# update packages and install maven
RUN  \
  export DEBIAN_FRONTEND=noninteractive && \
  sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y maven ssh sudo curl 

# Jenkins user
RUN useradd -m -s /bin/bash jenkins && \
    mkdir -p /home/jenkins/workspace && \
    chown jenkins:jenkins /home/jenkins/workspace && \
    mkdir -p /home/jenkins/.ssh && \
    chown jenkins:jenkins /home/jenkins/.ssh && \
    echo jenkins:jenkins | chpasswd && \
    echo "jenkins ALL=(root) NOPASSWD: ALL" > /etc/sudoers.d/jenkins && \
    chmod 440 /etc/sudoers.d/jenkins

EXPOSE 22
CMD ["/usr/sbin/sshd.real", "-D"]
