# Pterodactyl Wings Development Environment
# Base: Debian Bookworm
# Maintainer: biraru@biraru.org
# Source: https://github.com/BiraruStudios/development
FROM debian:bookworm-slim

# Metadata
LABEL maintainer="biraru@biraru.org" \
    description="Local development environment for Pterodactyl Wings" \
    org.opencontainers.image.source="https://github.com/BiraruStudios/development"

# Environment variables
ENV DEBIAN_FRONTEND=noninteractive \
    LANG=C.UTF-8 \
    GOBIN=/usr/local/go/bin \
    PATH=$PATH:/usr/local/go/bin

# Set user as root
USER root

# Install system dependencies
RUN apt -y update && apt -y install \
    git curl sudo nano tar zip unzip \
    make gcc g++ gdb python3 python3-pip python-is-python3 \
    iputils-ping iproute2 net-tools software-properties-common gnupg2 \
    supervisor \
    && rm -rf /var/lib/apt/lists/*

# Install Docker CE for container management
RUN curl -sSL https://get.docker.com/ | CHANNEL=stable bash \
    && usermod -aG docker root

# Install Go 1.24.1 for building Wings and Go tooling
RUN curl -OL "https://go.dev/dl/go1.24.1.linux-$(dpkg --print-architecture).tar.gz" \
    && tar -C /usr/local -xzf "go1.24.1.linux-$(dpkg --print-architecture).tar.gz" \
    && chown -R root:root /usr/local/go \
    && rm -f go1.24.1.linux-*.tar.gz

# Install Delve debugger for Go
RUN /usr/local/go/bin/go install github.com/go-delve/delve/cmd/dlv@latest

# Set working directory for Wings
WORKDIR /home/root/wings

# Expose ports used by Wings
EXPOSE 8080 2022
