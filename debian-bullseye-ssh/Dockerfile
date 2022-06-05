FROM debian:11

ARG ROOT_PASSWORD=root
ARG JENKINS_LINUX_USER=jenkins
ARG JENKINS_LINUX_PASSWORD=jenkins

ENV LANG=fr_FR.UTF-8
ENV TZ="Europe/Paris"

RUN echo 'Acquire::https {  Verify-Peer "false"; Verify-Host "false"; }' > /etc/apt/apt.conf.d/ignore_ssl
 
RUN apt update && \
    apt install -y dos2unix curl wget vim nano openssh-server openssh-client ca-certificates locales git default-jdk libcap2-bin

RUN mkdir -p /run/sshd

RUN echo "root:${ROOT_PASSWORD}" | chpasswd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \ 
    sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config


RUN adduser ${JENKINS_LINUX_USER} && echo "${JENKINS_LINUX_USER}:${JENKINS_LINUX_PASSWORD}" | chpasswd

EXPOSE 22
 
CMD [ "/usr/sbin/sshd" , "-D" ]