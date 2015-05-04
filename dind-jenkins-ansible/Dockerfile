FROM jenkins
MAINTAINER ryan.walls@3dsim.com
# Credit goes to: Johan Haleby http://www.jayway.com/2015/03/14/docker-in-docker-with-jenkins-and-supervisord/ 

# Switch user to root so that we can install apps (jenkins image switches to user "jenkins" in the end)
USER root

# Install ansible dependencies (Perhaps move this into a separate container?)
RUN apt-get update
RUN apt-get install python-pip python-dev git -y
RUN pip install PyYAML jinja2 paramiko boto
RUN git clone https://github.com/ansible/ansible.git --recursive
RUN cd ansible && git checkout tags/v1.9.1-0.2.rc2 && . ./hacking/env-setup && make install
RUN pip install http://github.com/diyan/pywinrm/archive/master.zip#egg=pywinrm
# Mounting with a volume now
#COPY .ssh /root/.ssh

# Install AWS CLI
RUN pip install awscli

# Install Docker prerequisites
RUN apt-get update -qq && apt-get install -qqy \
    apt-transport-https \
    ca-certificates \
	lxc \
	supervisor

# Create log folder for supervisor, jenkins and docker
RUN mkdir -p /var/log/supervisor
RUN mkdir -p /var/log/docker
RUN mkdir -p /var/log/jenkins


# Install Docker from Docker Inc. repositories.
RUN echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list \
  && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9 \
  && apt-get update -qq \
  && apt-get install -qqy lxc-docker

# Add jenkins user to the docker groups so that the jenkins user can run docker without sudo
RUN gpasswd -a jenkins docker

# Install the magic wrapper
ADD ./wrapdocker /usr/local/bin/wrapdocker
RUN chmod +x /usr/local/bin/wrapdocker

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf
