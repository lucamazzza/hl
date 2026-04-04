FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PATH="/root/.local/bin:/usr/local/bin:$PATH"

# C/C++ Compilation
RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    g++ \
    make \
    cmake \
    ninja-build \
    libx11-dev \
    libgl1-mesa-dev \
    libxext-dev \
    && rm -rf /var/lib/apt/lists/*

# Debug & Profiling tools
RUN apt-get update && apt-get install -y \
    gdb \
    lldb \
    valgrind \
    strace \
    && rm -rf /var/lib/apt/lists/*

# Terminal & Shell utilities
RUN apt-get update && apt-get install -y \
    zsh \
    tmux \
    fzf \
    tree \
    curl \
    wget \
    git \
    unzip \
    tar \
    gzip \
    && rm -rf /var/lib/apt/lists/*

# Zoxide
RUN curl -sS https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | sh

# Oh-My-Posh
RUN curl -s https://ohmyposh.dev/install.sh | bash -s

# Neovim dependencies
RUN apt-get update && apt-get install -y \
    nodejs \
    npm \
    python3 \
    python3-pip \
    python3-venv \
    ripgrep \
    fd-find \
    && rm -rf /var/lib/apt/lists/*

# Latest neovim
RUN curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz \
    && tar -C /opt -xzf nvim-linux-x86_64.tar.gz \
    && ln -s /opt/nvim-linux-x86_64/bin/nvim /usr/local/bin/nvim \
    && rm nvim-linux-x86_64.tar.gz

RUN git clone https://github.com/lucamazzza/dotfiles.git /tmp/dotfiles \
    && mkdir -p /root/.config \
    && cp -r /tmp/dotfiles/.config/* /root/.config/ \
    && cp /tmp/dotfiles/.zshrc /root/.zshrc \
    && rm -rf /tmp/dotfiles \
    && sed -i 's|eval "$(/opt/homebrew/bin/brew shellenv)"|# brew removed for linux|' /root/.zshrc \
    && sed -i 's|eval "$(tmux attach)"|# tmux attach removed for linux|' /root/.zshrc \
    && sed -i '/java_home/d' /root/.zshrc

WORKDIR /workspace

SHELL ["/bin/zsh", "-c"]
ENV SHELL=/bin/zsh

RUN nvim --headless "+Lazy! sync"   +qa || true
RUN nvim --headless "+TSUpdateSync" +qa || true
RUN nvim --headless "+MasonUpdate"  +qa || true

CMD ["zsh"]
