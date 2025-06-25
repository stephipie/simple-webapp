FROM jenkins/jenkins:lts

USER root

# Node.js 24.x installieren
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm

# Optional: Nützliche Tools
RUN apt-get install -y git

USER jenkins
