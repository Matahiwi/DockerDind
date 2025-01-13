# Use the official Jenkins agent image as the base
FROM jenkins/inbound-agent:latest

# Install Docker
USER root
RUN apt-get update && apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common
RUN install -m 0755 -d /etc/apt/keyrings 
RUN curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
RUN chmod a+r /etc/apt/keyrings/docker.asc
RUN echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt-get update
RUN apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Configure Docker to run as non-root user
RUN groupadd docker
RUN usermod -aG docker jenkins

# Install Docker Compose
RUN curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

# Enable DinD
RUN mkdir -p /var/lib/docker
VOLUME /var/lib/docker
