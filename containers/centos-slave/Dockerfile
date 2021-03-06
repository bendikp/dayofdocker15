FROM centos:7

# Set the correct timezone:
ENV TZ=Europe/Oslo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Exposes port 22 for incoming SSH connections.
EXPOSE 22

## The following line is not needed on centos. It is needed on ubuntu.
## RUN mkdir /var/run/sshd


# Centos 7 provides git-1.8.3
# Need gcc-go to compile the simple web server in go language. (Day of Docker project)
RUN yum -y groupinstall "Development Tools" \
 && yum -y install wget tar cpan vim-minimal vim-enhanced csh openssh-server java-1.8.0-openjdk-headless \
      zlib-devel git gcc-go golang \
 && yum -y update && yum clean all 


# Setup SSH host keys, which are required for sshd to start properly.
RUN /usr/bin/ssh-keygen -q -t rsa1 -f /etc/ssh/ssh_host_key     -C '' -N '' >&/dev/null
RUN /usr/bin/ssh-keygen -q -t rsa  -f /etc/ssh/ssh_host_rsa_key -C '' -N '' >&/dev/null
RUN /usr/bin/ssh-keygen -q -t dsa  -f /etc/ssh/ssh_host_dsa_key -C '' -N '' >&/dev/null
RUN /usr/bin/ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -C '' -N '' >&/dev/null
RUN /usr/bin/ssh-keygen -q -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -C '' -N '' >&/dev/null

RUN useradd -m builduser \
  && echo "builduser:dayofdocker" | /usr/sbin/chpasswd

COPY dotgitconfig /home/builduser/.gitconfig


######################################################################################
# Use image as:
# sudo docker run --rm -it  dod/centos:7 bash

# Run the SSH service in the container:
# docker run -d -p 2222:22 dod/centos:7 /usr/sbin/sshd -D

# Build this image with: 
# sudo docker build --rm -t dod/centos:7 .


