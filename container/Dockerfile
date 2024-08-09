FROM ubuntu:20.04

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive
# Update the package repository and install packages
RUN apt-get update \
    && apt-get upgrade -y \
    && useradd -m agentuser \
    && apt-get install -y --no-install-recommends \
        apt-transport-https \
        apt-utils \
        ca-certificates \
        curl \
        git \
        iputils-ping \
        jq \
        lsb-release \
        software-properties-common \
    # Install Azure CLI
    && curl -sL https://aka.ms/InstallAzureCLIDeb | bash \
    # Install PowerShell
    && curl -sL https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/microsoft.list' \
    && apt-get update \
    && apt-get install -y powershell

# Clean up
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Define the TARGETARCH environment variable
# Can be 'linux-x64', 'linux-arm64', 'linux-arm', 'rhel.6-x64'.
ENV TARGETARCH=linux-x64

# Set the working directory to /azp
WORKDIR /azp

# Adjust permissions
RUN chown -R agentuser:agentuser /azp \
    && chmod 755 /azp

# Copy the start script
COPY ./start.sh .
RUN chmod +x start.sh

# Switch to the agentuser
USER agentuser

# Use the start script as the entry point
ENTRYPOINT [ "./start.sh", "--once" ]
