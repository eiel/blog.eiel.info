---

title: "通知環境としてidobataを試した話をLT駆動開発04でした"
date: 2014-06-07T23:34:00+09:00
comments: true
tags: ["LT駆動","bot"]
---

今月も[LT駆動開発04](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA04)でライトニングトークしてきました。

結構今回雑…。喋りもグダってた。見直しとか、リハっぽいこともしてないから…。

<script async class="speakerdeck-embed" data-id="b2e78510d07b0131bebd268a39862ac0" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

[CircleCI](https://circleci.com/)の通知をしたいなーってことで [idobata](https://idobata.io/) のクライアント使えばできそうなので、 [hubot](https://hubot.github.com/) をつかって idobata に情報を送信してみたよ!という話をしました。

なんか開発で BOT を使うの良さってのがやっとわかってきたので、その話もちらっとした気がします。しかし、中身がないな…。

気になってるのは CircleCI の Web Hook から飛んでくるJSONの情報が整理されてなくて調べるのがめんどくさいなーって思ってたりします。なんかライブラリはないのだろうか。

### 関連

* [hubot で起動しているウェブサーバを経由して idobata へ投稿してみる](http://blog.eiel.info/blog/2014/05/27/message-send-from-hubot-httpd-to-idobata/)
