FROM ubuntu:24.04
LABEL author="benjamin.yde@gmail.com"
ENV DEBIAN_FRONTEND=noninteractive

# Install Development APT Packages
RUN apt -qq update && \
    apt -qq install -y --no-install-recommends \
        adduser \
        apt-utils \
        bash-completion \
        build-essential \
        ca-certificates \
        curl \
        git \
        git-lfs \
        nano \
        sudo \
        unzip \
        tar \
        wget \
        zsh \
        && \
    apt clean && \
    apt autoclean && \
    apt remove && \
    apt autoremove -y && \
    rm -rf /var/lib/apt/lists

# Create User
ARG USER_UID=1000
RUN deluser --remove-home ubuntu && \
    useradd --create-home --user-group --groups sudo --uid ${USER_UID} developer && \
    echo "developer ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/developer && \
    chown -R developer:developer /home/developer
USER developer
WORKDIR /home/developer

# Install Rust
# see versions here: https://releases.rs/
ENV RUST_VERSION=1.88.0
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs -o rustup-init.sh && \
    sh rustup-init.sh -y --default-toolchain ${RUST_VERSION} && \
    rm rustup-init.sh

# Install ZSH
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" && \
    touch .zshrc && \
    echo '\n# ZSH' >> .zshrc && \
    echo "export ZSH=\"/home/developer/.oh-my-zsh\"" >> .zshrc && \
    echo "export ZSH_THEME=\"eastwood\"" >> .zshrc && \
    echo "plugins=(git)" >> .zshrc && \
    echo "source /home/developer/.oh-my-zsh/oh-my-zsh.sh" >> .zshrc

ENTRYPOINT ["/usr/bin/zsh"]