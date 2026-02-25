FROM ubuntu:22.04

# Avoid interactive prompts
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    python3-pip \
 && rm -rf /var/lib/apt/lists/*

RUN pip3 install ansible-core==2.18.7 --break-system-packages

RUN ansible-galaxy collection install cisco.ios