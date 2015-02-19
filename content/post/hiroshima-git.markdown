---

title: "広島Git勉強会 201306 - やりなおせるGit入門"
date: 2013-06-02T20:47:00+09:00
comments: true
tags: [git,勉強会]
---

[広島Git勉強会](http://local.aguuu.com/events/15354) に参加しました。

1セッション喋りました。

はじめから`git reset` と `git checkout`あたりを説明しようと思ってたのですが、「元に戻せること」を主眼においていろいろ考えました。
結果として、「危険」「少し危険」なコマンドを定義して、よくわからない時どうすればいいのか伝えられるか試みてみました。

「危険」なコマンドはワークツリーにした変更が消えてしまう恐れがあるもの。

「少し危険」なコマンドは`git reflog`などを利用しないと追えなくなるコミットができてしまうもの。

と定義して、そこを強調しながら説明してみました。

スライドはこちらに。

<iframe src="http://www.slideshare.net/slideshow/embed_code/22237343" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen webkitallowfullscreen mozallowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="http://www.slideshare.net/TomohikoHimura/git-22237343" title="やりなおせる Git 入門" target="_blank">やりなおせる Git 入門</a> </strong> from <strong><a href="http://www.slideshare.net/TomohikoHimura" target="_blank">Tomohiko Himura</a></strong> </div>

結局、難しかったのか簡単だったのか、周りの空気を読む余裕が僕にはまだまだ足りなくて、「経験値を積まないといけないなぁ」と、思うのでした。

## 追記 ブクマのコメントなどなどに返信

> 「git commit の -m はそろそろ卒業しましょう」というのはどういうことなのかな？

`-m` って解説のために、そう書いてることが多いじゃないかと思う。スライド上でも ` -m`を利用していますが、こ の場合はコミットログをタイトルしか書かない場合が予想できる。なので、「概要も書きましょう。」という話をするために書いています。

> 良いまとめ。だけど rm -rf .git ってそんなにカジュアルでいいのかなw

すごく危険な操作なので、カジュアルにやるのはよくないですが、初回のコミットまでなら。という感じで口頭では伝えております。スライドにも入れればよかった。

## 関連リンク

* [広島Git勉強会 - 番外編 Github Flow してみる](http://blog.eiel.info/blog/2013/06/02/hiroshima-git-extend/)
* [岡山Git勉強会 - 20130223 - Git 仕組み入門 という話をしてきた](http://blog.eiel.info/blog/2013/02/23/okagit-20130223/)
* [Git がわからなくても Github を利用しよう](http://blog.eiel.info/blog/2013/02/06/how-to-use-github/)
* [Github の楽しみ方](http://blog.eiel.info/blog/2013/05/13/how-to-enjoy-github/)
