
FROM ubuntu:focal

ENV USER linuxbrew
ENV HOME /home/linuxbrew
ENV PATH /home/${USER}/.linuxbrew/bin:$PATH

SHELL ["/bin/bash", "-c"]

# 基本的な開発環境構築
RUN apt-get update && \
    apt-get upgrade && \
    apt-get -y install build-essential procps curl file git sudo vim

RUN useradd -m ${USER} && \
    echo "${USER}:hoge" | chpasswd && \
    usermod -G adm ${USER} && \
    echo "${USER} ALL=(ALL) NOPASSWD:ALL" >>/etc/sudoers

# ユーザーとホームディレクトリを変更
USER ${USER}
WORKDIR ${HOME}

# HomeBrew環境構築
RUN git clone https://github.com/Homebrew/brew /home/${USER}/.linuxbrew/Homebrew && \
    mkdir /home/${USER}/.linuxbrew/bin && \
    ln -s /home/${USER}/.linuxbrew/Homebrew/bin/brew /home/${USER}/.linuxbrew/bin && \
    echo "eval $(/home/${USER}/.linuxbrew/bin/brew shellenv)" >>~/.profile

RUN brew doctor && \
    brew install git

# ユーザー環境
RUN echo "alias l='ls -alF'" >> .bash_aliases && \
    sudo apt-get -y install fish
