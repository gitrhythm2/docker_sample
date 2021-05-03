
FROM centos:8

ENV HOME /root
ENV PATH /home/linuxbrew/.linuxbrew/bin:$PATH

RUN yum -y groupinstall 'Development Tools' && \
    yum -y install procps-ng curl file git sudo

WORKDIR ${HOME}

RUN git clone https://github.com/Homebrew/brew /home/linuxbrew/.linuxbrew/Homebrew && \
    mkdir /home/linuxbrew/.linuxbrew/bin && \
    ln -s /home/linuxbrew/.linuxbrew/Homebrew/bin/brew /home/linuxbrew/.linuxbrew/bin && \
    eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv) && \
    echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile && \
    echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile && \
    echo "alias l='ls -alF'" >> .bashrc

RUN brew doctor && \
    brew install git
