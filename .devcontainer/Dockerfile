FROM nvcr.io/nvidia/l4t-base:r32.3.1

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
        git vim tmux build-essential bash-completion sudo && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
ENV USERNAME ubuntu
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME && \
    useradd -m $USERNAME --uid $USER_UID --gid $USER_GID --shell /bin/bash && \
    echo "$USERNAME:$USERNAME" | chpasswd && \
    usermod -aG sudo $USERNAME && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
    gpasswd -a $USERNAME video
RUN echo "export PATH=/usr/local/cuda/bin:$PATH" >> ~/.bashrc && \
    echo "export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH" >> ~/.bashrcUSER $USERNAME
USER $USERNAME

