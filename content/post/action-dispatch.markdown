---

title: "ActionDispatch ってなんだろう？ - 広島・岡山Ruby交流会01"
date: 2014-03-30T01:27:00+09:00
comments: true
tags: ["ActionDispatch"]
---

ちわっす。絶賛、仕事が遅れまくっていてこんなことをしている場合じゃない状態です。こんにちは。

[広島岡山Ruby交流会01](http://hirosimaokayamarb.doorkeeper.jp/events/8993) に参加してきました。
一応セッションをしたので、ブログに残しておきたいと思います。

Rails のコードリーディングをしているので、自分の中に構築できたイメージをアウトプットをしようというシリーズです。4ヶ月ぶりぐらいですね。

今回は ActionDispatch です。

<script async class="speakerdeck-embed" data-id="97426d70998c013115765a48c3b99610" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

ActionDispatch ってなにすんだよ？って正直思っていたので、「ActionDispatchってなんだろう？」というタイトルにしました。

Rails は MVC フレームワークということで MVC のどこかにはまるものであればイメージが湧きやすいのですが、それ意外の部分になると途端に想像できなくなります。
ようやく想像できるようになったので簡単に図示しつつ、それと気になる部分をざっくりと紹介しました。

スライド中のコードは Gist にアップロードしました。

[https://gist.github.com/eiel/b4c6e39bf9b694022b17](https://gist.github.com/eiel/b4c6e39bf9b694022b17)

最後に言い訳をしておくと、時間を使ってる場合ではないので図が雑です。

Rails の勉強をしている人に参考になれば幸いです。
