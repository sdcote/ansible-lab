# Ubuntu with Ansible and OpenSSH
#
# The idea is to have a local control node running into which you can SSH and 
# provision other running containers.
#
# Assuming you built this with the following command:
#
# docker build --no-cache -t ansible .
#
# You can run this
#
# docker run -d -P --name <container_name> <image_name>
#
# You can non SSH into the container and use ansible with root:ansible
#
# Consider using volumes for all your local playbooks and inventories so you 
# don't lose data when you remove the container.
#
# docker volume create ansible
# docker run -d -P -v ansible:/opt/ansible --name <container_name> <image_name>
# 
# Next, run one or more containers which are listening to SSH and have python 
# installed.
#

FROM ubuntu:18.04
LABEL maintainer Steve Cote <sdcote@gmail.com>

# Install Ansible
RUN apt-get update \
  && apt-get install -y software-properties-common \
  && apt-add-repository ppa:ansible/ansible \
  && apt-get update -y \
  && apt-get install ansible -y 

# Install the SSH daemon so we can log into this container and run our playbooks
RUN apt-get install -y openssh-server \
  && mkdir /var/run/sshd 

# Install some helper tools - git, vim, sudo and unzip
RUN apt-get install -y unzip git sudo vim 

# Add a non privileged user that Ansible can use for connections
RUN useradd -m ansible -s /bin/bash \
 && echo 'ansible:ansible' | chpasswd 

# Make sure the ansible user can sudo
RUN echo "ansible ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/ansible \
  && chmod 0440 /etc/sudoers.d/ansible

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
