# Use an official base image with your preferred Linux distribution
FROM node:16.10

ENV DEBIAN_FRONTEND noninteractive

ARG CHROME_VERSION
ENV CHROME_VERSION "${CHROME_VERSION:-114.0.5735.90-1}"
ARG CHROMEDRIVER_DIR
ENV CHROMEDRIVER_DIR "${CHROMEDRIVER_DIR:-/usr/bin}"
ARG CHROMEDRIVER_VERSION
ENV CHROMEDRIVER_VERSION "${CHROMEDRIVER_VERSION:-114.0.5735.90}"
ARG TERRAFORM_VERSION
ENV TERRAFORM_VERSION "${TERRAFORM_VERSION:-1.5.6}"

# Update the package lists and install essential packages
RUN apt-get update && \
    apt-get install -y \
    curl \
    wget \
    apt-transport-https \
    lsb-release \
    gnupg \
    unzip \
    software-properties-common \
    nodejs \
    python3 \
    python3-pip \
    python3-setuptools \
    groff \
    less \
    && pip3 --no-cache-dir install --upgrade pip \
    && apt-get clean \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN pip3 --no-cache-dir install --upgrade awscli

#Install Google-chrome
RUN apt-get update && \
    apt-get install -y \
    fonts-liberation \
    libasound2 \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libatspi2.0-0 \
    libcups2 \
    libdbus-1-3 \
    libdrm2 \
    libgbm1 \
    libgtk-3-0 \
    libnspr4 \
    libnss3 \
    libu2f-udev \
    libvulkan1 \
    libxcomposite1 \
    libxdamage1 \
    libxfixes3 \
    libxkbcommon0 \
    libxrandr2 \
    xdg-utils && \
    rm -rf /var/lib/apt/lists/*\
    \
    && echo "Installing Google Chrome ${CHROME_VERSION}" \
    \
    && curl -LO "https://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_${CHROME_VERSION}_amd64.deb" \
    && dpkg -i "google-chrome-stable_${CHROME_VERSION}_amd64.deb" \
    ;

#Install and setup chrome-driver
RUN curl -LO "https://chromedriver.storage.googleapis.com/${CHROMEDRIVER_VERSION}/chromedriver_linux64.zip" \
    && unzip chromedriver_linux64.zip  \
    && mv chromedriver ${CHROMEDRIVER_DIR} \
    && chown root:root ${CHROMEDRIVER_DIR} \
    && chmod +x ${CHROMEDRIVER_DIR}

ENV PATH ${CHROMEDRIVER_DIR}:$PATH
# Install pip for Python 3
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && \
    python3 get-pip.py && \
    rm get-pip.py

# Install Docker Compose
RUN curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

# Install Terraform
RUN curl -LO "https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip" && \
    unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    mv terraform /usr/local/bin/ && \
    rm terraform_${TERRAFORM_VERSION}_linux_amd64.zip

# Cleanup
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*
