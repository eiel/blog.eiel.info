---

title: "magit-brame-modeの表示がみやすかった件"
date: 2012-05-30T17:55:00+09:00
comments: true
categories: [emacs,magit,git]
---
共同作業ないし、自分が書いたコードでも**ここ変更したのいつだっけ**ってことあると`git blame`を利用するんですが、emacs の magit についてる magit-brame-mode がみやすくて感動しました。

`git blame`がどのようなことをしてくれるのかは github でも特定のファイルを選択したときにもみることができます。

![github blame](http://media.tumblr.com/tumblr_m4th98QljM1qk2e7q.jpg)

押すとこんな感じ。

![github de blame](http://media.tumblr.com/tumblr_m4thgjx4eq1qk2e7q.jpg)

すっきりみやすい。

端末で `$ git blame conf/50-ruby.el` するとこんな感じ。

![端末](http://media.tumblr.com/tumblr_m4thlzshOW1qk2e7q.jpg)

文字ばっかり。

git-emacsの `M-x git-blame-mode` だとこんな感じ

![git-emacs](http://media.tumblr.com/tumblr_m4throQoHf1qk2e7q.jpg)

カラフル。細かい情報はミニバファにでます。そのまま編集できるのはナイスなのかもしれない。(重いですけど

magitの `M-x magit-blame-mode` だとこんな感じ

![magit](http://media.tumblr.com/tumblr_m4thwdwoK61qk2e7q.jpg)

あれ、なんかみやすさが伝わない…。
80列におさまるのは嬉しいですね。

ゆっくりみる分には github が一番いんじゃないかということいわれてしまいそうです。
