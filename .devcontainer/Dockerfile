# Start from the official .NET 9 dev container image
FROM mcr.microsoft.com/devcontainers/dotnet:9.0

# Set versions for Android tools as arguments
ARG ANDROID_CMD_LINE_TOOLS_VER="11479570"
ARG ANDROID_BUILD_TOOLS_VER="34.0.0"
ARG ANDROID_PLATFORM_VER="34"

# Switch to root user to install tools
USER root

# Install system dependencies
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends ca-certificates curl gnupg \
    && curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    # CHANGE: Use the correct package name for OpenJDK
    && apt-get -y install --no-install-recommends git nodejs unzip openjdk-17-jdk \
    # Clean up apt-get cache
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Set up environment variables for Android and Java
ENV ANDROID_SDK_ROOT="/usr/lib/android-sdk"
# CHANGE: Update JAVA_HOME to the correct path for version 17
ENV JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"
ENV PATH="$PATH:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools"

# Download and install the Android SDK
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools \
    && curl -o android_tools.zip "https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_CMD_LINE_TOOLS_VER}_latest.zip" \
    && unzip -q android_tools.zip -d "${ANDROID_SDK_ROOT}/cmdline-tools" \
    && mv "${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools" "${ANDROID_SDK_ROOT}/cmdline-tools/latest" \
    && rm android_tools.zip \
    # Accept licenses and install platforms/tools
    && yes | sdkmanager --licenses > /dev/null \
    && sdkmanager "platform-tools" "platforms;android-${ANDROID_PLATFORM_VER}" "build-tools;${ANDROID_BUILD_TOOLS_VER}"

# Install the required .NET workloads
RUN dotnet workload install android wasm-tools

# Install .NET global tools
RUN dotnet tool install -g dotnet-ef

# Switch back to the non-root 'vscode' user and fix permissions
USER vscode
RUN sudo chown -R vscode:vscode ${ANDROID_SDK_ROOT}
