---

title: "GitHub で SSH 接続できなくなった。SSH をつかった場合に高速化する設定が原因だった。"
date: 2013-11-09T14:05:00+09:00
comments: true
tags: ["GitHub"]
---

さっき、GitHub に push しようとしたら下記のエラーが発生した。

```
no matching cipher found: client arcfour256 server aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc
```

「`arcfour256` に対応してねーよ」ってことが書かれている。

`~/.ssh/config` を確認したら

```
Host github.com
  Compression yes
  Ciphers arcfour256
```

思いっきり自分で指定しています。

設定した記憶もないし、`arcfour256` ってなんだっけなとググると「GitHub で ssh をつかっていると遅くなるから、こういう設定したら速くなるよ」という記事がでてきた。

* [GitHub で clone するときは SSH じゃなく HTTP を使ったほうが高速 - てっく煮ブログ](http://tech.nitoyon.com/ja/blog/2013/01/11/github-clone-http/)

「なるほど、設定をコピペしたから記憶に残ってないんだ」と、思った。
コピペした設定は、ちゃんと勉強した上で使わないとダメですね。

そんな感想を抱いた。
