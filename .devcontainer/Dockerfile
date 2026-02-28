FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04

# Install system dependencies
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        git \
        pkg-config \
        libssl-dev \
        libsndfile1-dev \
        ffmpeg \
        python3-dev \
        clang \
        cmake \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js 18.x
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Switch to vscode user
USER vscode

# Install Rust with the required nightly toolchain
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- \
        --default-toolchain nightly-2022-12-18 \
        --profile minimal \
        -y \
    && ~/.cargo/bin/rustup component add rustfmt --toolchain nightly-2022-12-18

ENV PATH="/home/vscode/.cargo/bin:${PATH}"

# Install pyenv and Python 3.10.5
RUN curl https://pyenv.run | bash
ENV PYENV_ROOT="/home/vscode/.pyenv"
ENV PATH="${PYENV_ROOT}/bin:${PYENV_ROOT}/shims:${PATH}"
RUN pyenv install 3.10.5 && pyenv global 3.10.5

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 - \
    && ~/.local/bin/poetry config virtualenvs.in-project true

ENV PATH="/home/vscode/.local/bin:${PATH}"
