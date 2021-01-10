FROM nvcr.io/nvidia/l4t-base:r32.3.1

ARG USERNAME=nvidia
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID --shell /bin/bash -m $USERNAME
RUN echo $USERNAME:$USERNAME | chpasswd && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get install -yq wget curl git build-essential vim sudo lsb-release locales bash-completion 