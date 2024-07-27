FROM python:3.11-slim

# Install required tools and dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        git \
        openssh-client \
        procps \
        lsb-release \
        apt-transport-https \
        ca-certificates \
        gnupg \
        software-properties-common \
    && rm -rf /var/lib/apt/lists/*

# Install Docker CLI
RUN curl -fsSL https://get.docker.com | sh

# Install Docker Compose
RUN curl -sSL https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose

# Install Azure CLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash

# Set up non-root user
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Switch to non-root user
USER $USERNAME

# Set the default shell to bash
ENV SHELL=/bin/bash