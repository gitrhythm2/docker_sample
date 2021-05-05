
FROM centos:8

ENV USER linuxbrew
ENV HOME /home/linuxbrew
ENV PATH /home/${USER}/.linuxbrew/bin:$PATH

# 基本的な開発環境構築
RUN yum -y groupinstall 'Development Tools' && \
    yum -y install procps-ng curl file git sudo && \
    adduser ${USER} && \
    echo "${USER}:hoge" | chpasswd && \
    usermod -G wheel ${USER} && \
    echo "${USER} ALL=(ALL) NOPASSWD:ALL" >>/etc/sudoers

# ユーザーとホームディレクトリを変更
USER ${USER}
WORKDIR ${HOME}

# HomeBrew環境構築
RUN git clone https://github.com/Homebrew/brew /home/${USER}/.linuxbrew/Homebrew && \
    mkdir /home/${USER}/.linuxbrew/bin && \
    ln -s /home/${USER}/.linuxbrew/Homebrew/bin/brew /home/${USER}/.linuxbrew/bin && \
    eval $(/home/${USER}/.linuxbrew/bin/brew shellenv) && \
    echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile && \
    echo "alias l='ls -alF'" >> .bashrc && \
    brew doctor

RUN brew install git
