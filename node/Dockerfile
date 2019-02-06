#
# Assuming you built this mage with: 
#
# docker build --no-cache -t node  .
#
# The recommended way to run this for a regular node is:
# 
# docker run -d -p :22 --name nodea --hostname nodea node
#
# or if you want both the SSH and HTTP ports published:
#
# docker run -d -P --name nodea --hostname nodea node
#
FROM ubuntu:18.04
LABEL maintainer Steve Cote <sdcote@gmail.com>

COPY proxy.conf /etc/apt/apt.conf.d/proxy.conf

# Add the minimal packages for an Ansible to connect and manage our configuration
RUN apt-get update \
  && apt-get install -y openssh-server sudo python2.7 python-pip python3-pip \
  && mkdir /var/run/sshd 

# Add a non privileged user that Ansible can use for connections
RUN useradd -m ansible -s /bin/bash \
 && echo 'ansible:ansible' | chpasswd 

# Make sure the ansible user can sudo
RUN echo "ansible ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/ansible \
  && chmod 0440 /etc/sudoers.d/ansible

# Expose ssh and common ports.
EXPOSE 22 80 443

# Run SSH daemon
CMD ["/usr/sbin/sshd", "-D"]