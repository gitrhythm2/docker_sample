
FROM gitrhythm/ubuntu_brew:latest

SHELL ["/bin/bash", "-c"]

# nodenvのインストール
RUN brew install nodenv && \
    echo 'eval "$(nodenv init -)"' >> ~/.profile

# nodeのインストール
RUN eval "$(nodenv init -)" && \
    curl -fsSL https://github.com/nodenv/nodenv-installer/raw/master/bin/nodenv-doctor | bash && \
    nodenv install 16.1.0 && \
    nodenv local 16.1.0
