## 概要
centOS8にhomebrewを構築した`gitrhythm/centos8_brew:latest`をベースとして、rbenvでRuby環境を構築する。

## このサンプルで学んだこと


- [github - rbenv](https://github.com/rbenv/rbenv#understanding-path)
- [RubyをHomebrewのrbenvでインストールする手順](https://weblabo.oscasierra.net/ruby-install-rbenv-homebrew-macos/)

## このサンプルで学んだこと
環境変数の設定などは、RUNが別れていると引き継がれない.
```
# 1つ目
RUN ... && \
    'eval "$(rbenv init -)"'

# 2つ目
RUN ...
```
この時2つ目では、1つ目で設定した`eval...`が引き継がれない
RUNをまとめて1つにするか、2つ目でも改めて`eval...`を実行する
```
# 1つ目
RUN ... && \
    'eval "$(rbenv init -)"'

# 2つ目
RUN ... && \
    'eval "$(rbenv init -)"'
```

## 作業ログ
Dockerfile作成に先立ち、手動でRuby環境を構築してみたメモ。

### rbenvインストール
```
$ brew install rbenv
...
==> ruby-build
ruby-build installs a non-Homebrew OpenSSL for each Ruby version installed and these are never upgraded.

To link Rubies to Homebrew's OpenSSL 1.1 (which is upgraded) add the following
to your /home/linuxbrew/.bash_profile:
  export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)"

Note: this may interfere with building old versions of Ruby (e.g <2.4) that use
OpenSSL <1.1.
```

OpenSSLに関するメッセージが表示されていたので、言われた通りにしてみる
ついでにrbenv initの指定もしておく
```
$ sudo yum install -y openssl-devel
$ echo export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)" >> .bash_profile
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

ちなみに、`rbenv init`を実行すると`which rbenv`コマンドの実行結果が変わる
```
$ which rbenv
~/.linuxbrew/bin/rbenv

$ rbenv init -
export PATH="/home/linuxbrew/.rbenv/shims:${PATH}"
export RBENV_SHELL=bash
source '/home/linuxbrew/.linuxbrew/Cellar/rbenv/1.1.2/libexec/../completions/rbenv.bash'
command rbenv rehash 2>/dev/null
rbenv() {
  local command
  command="${1:-}"
  if [ "$#" -gt 0 ]; then
    shift
  fi

  case "$command" in
  rehash|shell)
    eval "$(rbenv "sh-$command" "$@")";;
  *)
    command rbenv "$command" "$@";;
  esac
}

# ログインし直して実行
$ which rbenv
rbenv ()
{ 
    local command;
    command="${1:-}";
    if [ "$#" -gt 0 ]; then
        shift;
    fi;
    case "$command" in 
        rehash | shell)
            eval "$(rbenv "sh-$command" "$@")"
        ;;
        *)
            command rbenv "$command" "$@"
        ;;
    esac
}

$ command which rbenv
/home/linuxbrew/.linuxbrew/bin/rbenv
```

[which rbenv in terminal returns function definition of rbenv #418](https://github.com/rbenv/rbenv/issues/418)に、この件について話されている。
`command which rbenv`で`/usr/bin/whici`が実行される。(それが面倒ならさらにbashを起動する手もある)

### Rubyインストール
```
# そもそもRubyはインストールされていない
$ rbenv versions
Warning: no Ruby detected on the system

$ rbenv install -l
2.6.7
2.7.3
3.0.1
(snip...)

$ rbenv install 3.0.1
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").

# エラー
ERROR: Ruby install aborted due to missing extensions
Try running `yum install -y zlib-devel` to fetch missing dependencies.

# zlibをインストール
$ sudo yum install -y zlib-devel

# 再度Rubyをインストール
$ rbenv install 3.0.1

# インストールされているが、デフォルトのRubyが設定されていない状態
$ rbenv versions
  3.0.1

$ rbenv global 3.0.1
$ rbenv versions
  * 3.0.1 (set by /home/linuxbrew/.rbenv/version)
$ ruby -v
  ruby 3.0.1p64 (2021-04-05 revision 0fb782ee38) [x86_64-linux]
```
