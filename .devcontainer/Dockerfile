FROM ubuntu:22.04

ARG CONTAINER_USER="developer"
ARG HOST_USER=1000
ARG HOST_GROUP=1000

# Update container
RUN apt update
# Install compiler and base tools
RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt install -y \
    bash-completion \
    bmap-tools \ 
    build-essential \
    ca-certificates \
    cmake \
    crossbuild-essential-arm64 \
    curl \
    fdisk \
    file \
    g++ \
    g++-aarch64-linux-gnu \
    gcc \
    gcc-aarch64-linux-gnu \
    gdb-multiarch \
    gdisk \
    git \
    gnupg \
    iproute2 \
    iptables \
    iputils-ping \
    locales \
    lshw \
    parted \
    pkg-config \
    python3 \
    qemu-system-arm \
    qemu-system-x86 \
    rsync \
    software-properties-common \
    squashfs-tools \
    sudo \
    tcpdump \
    tzdata \
    vim \
    wget \
    xz-utils \
    zip

# Setup locale
RUN export LC_ALL=en_US.UTF-8 && \
    export LANG=en_US.UTF-8 && \
    locale-gen en_US.UTF-8

# Create build user
# and allow user to use sudo
RUN userdel ubuntu || true
RUN addgroup --gid $HOST_GROUP $CONTAINER_USER && \
  useradd -rm -d /home/$CONTAINER_USER -s /bin/bash -g $CONTAINER_USER -G sudo -u $HOST_USER $CONTAINER_USER && \
  echo "$CONTAINER_USER ALL=(ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers && \
  mkdir -p /home/$CONTAINER_USER/.ssh && \
  chown -R $CONTAINER_USER:$CONTAINER_USER /home/$CONTAINER_USER

# Create mount point for SSH keys
RUN mkdir -p /home/$CONTAINER_USER/.ssh && \
    chown -R $CONTAINER_USER:$CONTAINER_USER /home/$CONTAINER_USER && \
    chmod 700 /home/$CONTAINER_USER/.ssh

# Create mount point for gnupg keys
RUN mkdir -p /home/$CONTAINER_USER/.gnupg && \
    chown -R $CONTAINER_USER:$CONTAINER_USER /home/$CONTAINER_USER && \
    chmod 700 /home/$CONTAINER_USER/.gnupg

# Install Yocto dependencies
RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt install -y \
    build-essential \
    chrpath \
    cpio \
    debianutils \
    diffstat \
    file \
    gawk \
    gcc \
    git \
    iputils-ping \
    libacl1 \
    liblz4-tool \
    locales \
    python3 \
    python3-git \
    python3-jinja2 \
    python3-pexpect \
    python3-pip \
    python3-subunit \
    socat \
    texinfo \
    unzip \
    wget \
    xz-utils \
    zstd


USER $CONTAINER_USER
WORKDIR /home/$CONTAINER_USER

# Install uv Python package manager
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
# Add uv to path and kas using uv
RUN bash -c "source $HOME/.local/bin/env && uv tool install kas"

# Install rustup
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
# Add cargo to path and install ripgrep
RUN bash -c "source $HOME/.cargo/env && cargo install ripgrep"
