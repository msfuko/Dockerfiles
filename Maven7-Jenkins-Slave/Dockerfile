FROM java:7

# update packages and install maven
RUN  \
  export DEBIAN_FRONTEND=noninteractive && \
  sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y git maven ssh sudo curl 

# Jenkins user
RUN useradd -m -s /bin/bash jenkins && \
    mkdir -p /home/jenkins/workspace && \
    chown jenkins:jenkins /home/jenkins/workspace && \
    mkdir -p /home/jenkins/.ssh && \
    chown jenkins:jenkins /home/jenkins/.ssh && \
    echo jenkins:dcsrd123 | chpasswd && \
    echo "jenkins ALL=(root) NOPASSWD: ALL" > /etc/sudoers.d/jenkins && \
    chmod 440 /etc/sudoers.d/jenkins

# Tmpfs: we divert because some old docker plugin version have /usr/sbin/sshd hard-coded
RUN mkdir -p /var/run/sshd && \
    dpkg-divert --add --rename --divert /usr/sbin/sshd.real /usr/sbin/sshd
ADD wrapsshd /usr/sbin/sshd
RUN chmod +x /usr/sbin/sshd

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
