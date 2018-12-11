# Ubuntu with Ansible and OpenSSH
#
# The idea is to have a local control node running into which you can SSH and 
# provision other running containers.
#
# Assuming you built this with the following command:
#
# docker build --no-cache -t control .
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
  && apt-get install -y unzip git software-properties-common \
  && apt-add-repository ppa:ansible/ansible \
  && apt-get update -y \
  && apt-get install ansible -y \
  && ssh-keygen -t rsa -b 4096 -N "" -f ~/.ssh/id_rsa

# Install the SSH daemon so we can log into this container and run our playbooks
RUN apt-get install -y openssh-server \
  && mkdir /var/run/sshd \
  && echo 'root:ansible' | chpasswd \
  && sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
  && sed -i 's/#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config

# This also installs Terraform - should probably put this in a separate Dockerfile
RUN wget --quiet https://releases.hashicorp.com/terraform/0.11.10/terraform_0.11.10_linux_amd64.zip \
  && unzip -qq terraform_0.11.10_linux_amd64.zip \
  && mv terraform /usr/local/bin/ \
  && rm terraform_0.11.10_linux_amd64.zip

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
