# UEFI開発用Dockerfile
FROM rust:1.82-slim

# 必要なパッケージをインストール（ビルド専用）
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    build-essential \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# UEFIターゲットを追加
RUN rustup target add x86_64-unknown-uefi

# 作業ディレクトリを設定
WORKDIR /workspace

# ユーザーを作成（オプション、ファイル権限の問題を避けるため）
ARG USERNAME=developer
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Rustツールチェーンをユーザーにも設定
USER $USERNAME
RUN rustup target add x86_64-unknown-uefi

# デフォルトコマンド
CMD ["bash"]