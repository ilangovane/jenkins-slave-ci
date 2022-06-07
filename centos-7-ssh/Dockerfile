FROM centos:7

# Arguments - default values
ARG ROOT_PASSWORD 

# Local settings 
ENV LANG=fr_FR.UTF-8
ENV LC_ALL=fr_FR.UTF-8
ENV LANGUAGE=fr_FR:fr_FR

# Install packages
RUN yum update -y && \
    yum install -y dos2unix curl wget vim nano openssh-server openssh-clients ca-certificates java-11-openjdk-devel

# Configuration SSHD
RUN mkdir -p /run/sshd

RUN ssh-keygen -P "" -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -q -N ""

RUN ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -q -N ""
RUN ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -q -N ""

RUN echo "root:${ROOT_PASSWORD}" | chpasswd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \ 
    sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Expose SSH port
EXPOSE 22

# Run sshd at boot 
CMD [ "/usr/sbin/sshd" , "-D" ]