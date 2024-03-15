# jenkins/ssh-agent image requires the eventual container to run as root.
# This is so that it can set up an SSH server in the container.
# It is generally considered UNSAFE to run a container as root, but
# this setup requires it.

FROM jenkins/ssh-agent:debian-jdk17

USER root

# Install enough of Docker to utilize the CLI
RUN update-ca-certificates && \
   apt-get update && \
   apt-get install curl -y && \
   install -m 0755 -d /etc/apt/keyrings && \
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc && \
   chmod a+r /etc/apt/keyrings/docker.asc && \
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian bullseye stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
   apt-get update && \
   apt-get install docker-ce docker-ce-cli -y &&\
   usermod -aG docker jenkins && \
   apt-get clean

USER jenkins

# Create a directory to store the docker client certs in
RUN mkdir -p /home/jenkins/.docker_certs

# Copy in the required files to use the Docker API on the host.
# You could possibly mount these as a volume if you are mindful
# of your file permissions.
# This is related to: https://docs.docker.com/engine/security/protect-access/
COPY ca.pem /home/jenkins/.docker_certs/ca.pem
COPY cert.pem /home/jenkins/.docker_certs/cert.pem
COPY key.pem /home/jenkins/.docker_certs/key.pem

USER root

# Make sure jenkins owns the .docker_certs files
RUN chown -R jenkins:jenkins /home/jenkins/.docker_certs

USER jenkins

# Create and set the docker context to use the host's Docker API
RUN docker context create jenkins-docker-context \
  --description "jenkins-docker-context" \
  --docker "host=tcp://host.docker.internal:2376,ca=/home/jenkins/.docker_certs/ca.pem,cert=/home/jenkins/.docker_certs/cert.pem,key=/home/jenkins/.docker_certs/key.pem" && \
  docker context use jenkins-docker-context

# Set back as root so that the container spins up properly when
# you create it.
USER root