# Base image shipped with Codespaces
FROM mcr.microsoft.com/vscode/devcontainers/base:ubuntu-22.04

# Install APT prerequisites
RUN apt-get update && apt-get install -y --no-install-recommends \
        ca-certificates \
        gnupg2 \
        wget \
        unzip \
        git \
        default-jdk-headless \
        curl \
        build-essential \
        openjdk-17-jdk \
        && rm -rf /var/lib/apt/lists/*

# Install Gradle (required for build)
ENV GRADLE_VERSION 8.3
RUN wget https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-all.zip -P /tmp && \
    unzip /tmp/gradle-${GRADLE_VERSION}-all.zip -d /opt && \
    ln -s /opt/gradle-${GRADLE_VERSION} /opt/gradle && \
    rm /tmp/gradle-${GRADLE_VERSION}-all.zip
ENV PATH="/opt/gradle/bin:${PATH}"

# Install Android SDK command‑line tools
ENV ANDROID_SDK_ROOT /usr/local/android-sdk
RUN mkdir -p $ANDROID_SDK_ROOT && \
    cd $ANDROID_SDK_ROOT && \
    wget https://dl.google.com/android/repository/commandlinetools-linux-8512546_latest.zip -O cmdline-tools.zip && \
    unzip cmdline-tools.zip && \
    rm cmdline-tools.zip && \
    mkdir cmdline-tools && \
    mv cmdline-tools/bin cmdline-tools/.latest/bin && \
    mv cmdline-tools/lib cmdline-tools/.latest/lib && \
    mv cmdline-tools/*.zip cmdline-tools/.latest/

# Accept licenses and install core platforms
RUN yes | sdkmanager --licenses
RUN sdkmanager "platform-tools" "platforms;android-33" "build-tools;33.0.3"

# Clean up
RUN apt-get purge -y wget unzip && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

# Verify installs
RUN java -version && gradle --version && sdkmanager --version
