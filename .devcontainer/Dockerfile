# Start from the official .NET 9 dev container image
FROM mcr.microsoft.com/devcontainers/dotnet:9.0

# Switch to root user to install tools
USER root

# Install system dependencies: Git for source control, Node.js/npm for Uno.Wasm tooling
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends git nodejs npm \
    # Clean up apt-get cache
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install .NET global tools
RUN dotnet tool install -g dotnet-ef

# Switch back to the non-root 'vscode' user
USER vscode
