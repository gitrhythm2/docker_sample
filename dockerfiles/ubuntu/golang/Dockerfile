
FROM gitrhythm/ubuntu_brew:latest

SHELL ["/bin/bash", "-c"]

# # nodenvのインストール
RUN brew install goenv && \
    echo 'export GOENV_ROOT="$HOME/.goenv"' >> ~/.profile && \
    echo 'export PATH="$GOENV_ROOT/bin:$PATH"' >> ~/.profile && \
    echo 'eval "$(goenv init -)"' >> ~/.profile

# # golangのインストール
RUN eval "$(goenv init -)" && \
    goenv install 1.11.4 && \
    echo 'export GOPATH="/home/linuxbrew/go"' >> ~/.profile && \
    echo 'export PATH="$GOROOT/bin:$PATH"' >> ~/.profile && \
    echo 'export PATH="$PATH:$GOPATH/bin"' >> ~/.profile && \
    goenv global 1.11.4 && \
    go get -u github.com/cosmtrek/air
