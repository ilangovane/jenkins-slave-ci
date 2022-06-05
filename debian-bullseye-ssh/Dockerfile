FROM debian:11

# Arguments - default values
ARG ROOT_PASSWORD=root
ARG JENKINS_LINUX_USER=jenkins
ARG JENKINS_LINUX_PASSWORD=jenkins

# Local settings 
ENV LANG=fr_FR.UTF-8
ENV TZ="Europe/Paris"

# Ignore SSL
RUN echo 'Acquire::https {  Verify-Peer "false"; Verify-Host "false"; }' > /etc/apt/apt.conf.d/ignore_ssl

# Install packages
RUN apt update && \
    apt install -y dos2unix curl wget vim nano openssh-server openssh-client ca-certificates locales git default-jdk libcap2-bin sudo maven

# Configuration SSHD
RUN mkdir -p /run/sshd

RUN echo "root:${ROOT_PASSWORD}" | chpasswd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \ 
    sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Configuration user for jenkins build
RUN adduser ${JENKINS_LINUX_USER} && echo "${JENKINS_LINUX_USER}:${JENKINS_LINUX_PASSWORD}" | chpasswd
RUN usermod -aG sudo ${JENKINS_LINUX_USER}
RUN echo "${JENKINS_LINUX_USER} ALL=(ALL: ALL) ALL" >> /etc/sudoers

# Expose SSH port
EXPOSE 22

# Run sshd at boot 
CMD [ "/usr/sbin/sshd" , "-D" ]