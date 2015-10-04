---
date: 2015-10-04T11:44:54+09:00
title: "Dockerコンテナの中からDockerコンテナを動かす話をした - LT駆動19"
tags: ["docker"]
---

[LT駆動開発19](https://github.com/LTDD/Sessions/wiki/LT駆動開発19)で「Docker >>= Docker」という話した。
「どっかーばいんどどっかー」と読む。
cronの中でDockerコマンドがつかいたかった。そのcrondはDockerホストでは当然動かしたくない。Dockerコンテナ内におきたい。
Remote APIでたたいてもいいけど、ちょっとしたものならコマンドが楽でよい。

dockerが使えるイメージはdockerがある。docker run サブコマンドは

```
$ docker run イメージ コマンド
```

なので、イメージを節約して、dockerイメージをつかいまわすと、
```
$ docker run docker docker run docker ls`
```
という不思議な呪文になった。
実際はDockerfileでRUNで指定したりするので、

```
$ docker run docker docker run hoge
```
とかですんだり、そもそも`docker run hoge`で済むようにつくることになるだろう。

ちなみに
```
$ docker run docker "docker run docker ls"`
```
だとうまくうごかない。

```
$ docker run docker bin/sh -c "docker run docker ls"
```
とすると良い。

`sh -c` の中でdockerコマンドで複数のプログラムをうごかしてパイプで処理をつないだりもできる。
そういう話をしました。

あと一番大事なことだけど、Docker Hostが動いているコンピュータ上のdockerコマンドは`/var/run/docker.sock`をつかって通信しているようなので、`-v /var/run/docker.sock:/var/run/docker.sock`を共有してやると、コンテナ内からHostと簡単に通信できる。
これって安全なのかはよくしらない。

Dockerの中でDockerを起動すると Docker Docker A になりそうだけど、Docker A になるので、 「Docker >>= Docker」というタイトルにしてみました。

スライドは即興でつくったので雑です。

<script async class="speakerdeck-embed" data-id="c9b2baf5752447559ac85dd46a1555ef" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
