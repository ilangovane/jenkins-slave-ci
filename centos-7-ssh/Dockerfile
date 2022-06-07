FROM centos:7

ARG ROOT_PASSWORD 

ENV LANG=fr_FR.UTF-8
ENV LC_ALL=fr_FR.UTF-8
ENV LANGUAGE=fr_FR:fr_FR

RUN yum update -y && \
    yum install -y dos2unix curl wget vim nano openssh-server openssh-clients ca-certificates java-11-openjdk-devel

RUN mkdir -p /run/sshd

RUN ssh-keygen -P "" -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -q -N ""

RUN ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -q -N ""
RUN ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -q -N ""

RUN echo "root:${ROOT_PASSWORD}" | chpasswd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \ 
    sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config

EXPOSE 22

CMD [ "/usr/sbin/sshd" , "-D" ]