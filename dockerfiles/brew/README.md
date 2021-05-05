## 概要
centOS8をベースとしてhomebrew環境を構築する。

- ユーザー「linuxbrew」を作成し、linuxbrewアカウント配下にhomebrew環境を構築する
- gitをhomebrew経由でインストールする 〜 yumよりも新しいバージョンのgitを入れたい

[Homebrew on Linux](https://docs.brew.sh/Homebrew-on-Linux)の「Alternative Installation」に沿った方法で環境を構築することとする。
(通常のやり方(Curl経由でのインストール)では、インストールの過程で端末からの入力を求める部分があってうまく行かなかった)

Docker Hubの「linuxbrew/linuxbrew」では、また別のアプローチで環境を構築しているが、内部の仕様を理解していないとできないように感じたので、ここではhomebrewをgit cloneする方法でやることとする。

## このサンプルで学んだこと

- イメージはRUNコマンド毎に作成され、一度作成するとキャッシュされる
- USER,WORKDIRでRUNコマンドを実行するユーザーとディレクトリを指定することができる 〜 RUNコマンドの中で「su - linuxbrew」のようなことをやってみたがうまく行かなかった(何かやり方が間違っていたのかもしれないが、USER,WORKDIRで指定できるならば、その方が明確なのかも知れない)
- `docker exec`でbashを実行する際、`login`を付与しないと.bash_profileを読み込んでくれない 〜 読み込んでくれないとPATHの設定がされない

```
$ docker exec -it #{container} /bin/bash          # これだと.bash_p路ふぃぇを読み込まない
$ docker exec -it #{container} /bin/bash --login  # --loginを付与すると読み込む
```

- `ENV PATH /home/${USER}/.linuxbrew/bin:$PATH`をDockerfileに定義しているが、これがないと`brew doctor`の部分でbrewを参照できなくてエラーになる(他に良い方法がありそうな気もするが、今は追求していない)
