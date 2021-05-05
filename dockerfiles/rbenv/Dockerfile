
FROM gitrhythm/centos8_brew:latest

# gitrhythm/centos8_brewから引き継いだ環境
# USER linuxbrew
# HOME /home/linuxbrew

# rubyenvのインストール
RUN brew install rbenv && \
    brew install openssl@1.1 && \
    echo 'eval "$(rbenv init -)"' >> ~/.bash_profile && \
    echo export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)" >> .bash_profile

# Rubyのインストール
RUN eval "$(rbenv init -)" && \
    export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)" && \
    sudo yum install -y zlib-devel && \
    rbenv install 3.0.1 && \
    rbenv global 3.0.1
