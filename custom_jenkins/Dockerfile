FROM jenkins/jenkins:lts

USER root

# Install only supporting tools (not Docker engine or CLI)
RUN apt-get update -y && \
    apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release && \
    apt-get clean

# Create docker group and add Jenkins user to it
RUN groupadd -f docker && usermod -aG docker jenkins

USER jenkins
