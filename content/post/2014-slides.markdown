---

title: "2014年を振り返る - スライド編"
date: 2014-12-31T14:47:00+09:00
comments: true
tags: []
---

2014年をふりかえりしようと思っていたら31日なっていた。

今年は去年に引続きアウトプットは意識してやっているので分量が多い。
というわけで、分割しながらいきます。

まずは公開したスライドから。

今年はspeakerdeckにしかスライドをアップロードしていない。

https://speakerdeck.com/eiel

ざっと確認したところ34個のスライドを公開したようです。

途中からkeynoteのファイルも以下で全て公開しています。

http://keynotes.eiel.info/

### ざっくり

主に2014年3月からはじまった[LT駆動開発](http://ltdd.doorkeeper.jp/)のせいで毎月スライドを二つ作ったりしたせいで30越えになった。
分割したのに、とても振り返ることのできる分量ではない気がする。

仕事が忙しかった4月が一番少なく、一番仕事を避けていた9月が一番多い結果になった。
9月だけで8個あってわけがわからない。そういえば9月なんも仕事をした記憶がなかったのはこのせいだろう。

# いくつか紹介する

### オープンセミナー2014@広島の実行委員長になったので…完全版

主に2013年のふりかえりの結果を整理したスライド。
2013年はスライドを23個つくっていたことがわかった。

<script async class="speakerdeck-embed" data-id="d04a296071f801316b633e3e0c4ab66b" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

そいや今回のオープンセミナーの申し込みに事前アンケートをつくり忘れていることにも気づいた。
今回は私はただの下っ端ですが、目標は120人ぐらいなので、みんな申し込みしてください。

[オープンセミナー2015@広島 - オープンセミナー広島 | Doorkeeper](http://osh-web.doorkeeper.jp/events/18561)

### CIのある生活

第1回のLT駆動開発用に対象者をウェブをやっている人まで引き下げたつもりでつくったスライド。
しかし、イベントの時間があまらなかったので大幅縮小で実質お蔵入りである。

今みなおすと、長い。長いよ…。

<script async class="speakerdeck-embed" data-id="af93ee90836f0131cf5b265e09c13f13" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* [LT駆動開発で「CI のある生活」という話をするはずだった - そんなこと覚えてない](http://blog.eiel.info/blog/2014/03/03/ci-in-ltdd/)


### ActionDispatch ってなんだろう？

[広島岡山Ruby交流会 01](http://hirosimaokayamarb.doorkeeper.jp/events/8993)のためにつくったスライド。
RailsのActionDispatchがどういうものが自分でイメージできるようになっていたので整理した。

Railsのアーキテクチャは無駄にがんばったがあっているか謎だ。
コアにはrackがいて、小さなラックアプリケーションへ橋渡しをする大きなラックアプリケーションがActionDIspatchである。
ルーティングをしているだけともいえる。

<script async class="speakerdeck-embed" data-id="97426d70998c013115765a48c3b99610" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* [ActionDispatch ってなんだろう？ // Speaker Deck](https://speakerdeck.com/eiel/actiondispatch-tutenandarou)

### S3にスライドを保存することにした

スライドって一度つくったらなおすことないしS3に保存するかーってことではじめてAWSのAPIを叩いて感動したので、HaskellでS3のAPIを叩いて遊んだ話である。
PVはあまり伸びなかった。(あたり前)

<script async class="speakerdeck-embed" data-id="0917d000b4c40131d8ee7625813d8974" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* [LT駆動開発03で「S3にスライドを保存することにした」という発表者をしてきた。 - そんなこと覚えてない](http://blog.eiel.info/blog/2014/05/03/ltdd-03-s3/)

### 継続は力になるのか

結構反響があったスライドだけど、当日はあんまみんな話を聞いてなかった気がする記憶。

日々続けることで身につく力はすごいと思う。

途中にいれたツイートがおもったより伸びない。(仕方ない)

<script async class="speakerdeck-embed" data-id="3f7575c0c1940131c12706a532522dbf" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* [オープンセミナー2014@岡山に参加した。ついでに懇親会で継続することについてLTした。 - そんなこと覚えてない](http://blog.eiel.info/blog/2014/05/19/open-seminar-2014-at-okayama/)

### 愛無双 - エンジニアリングの楽しさ

DevLOVE広島 第一回でエンジニアリングの楽しさを語るという使命の元に作成したもの。
実はかなり悩んだので作るのにつかった時間はかなりかかっている。
しかし、全然伸びなかった子なので見てない人はみて欲しいところ。

We're so Happyをめざしたい。

<script async class="speakerdeck-embed" data-id="0c4650b0dca20131b4bb7abe6293b58c" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* [DevLOVE広島　第一回（夏の陣） でエンジニアリングの楽しさについて喋らされた - そんなこと覚えてない](http://blog.eiel.info/blog/2014/06/23/devlove-in-hiroshima-01/)

### 誰がおるんか

オフィスに誰がいるのかチャットツールでわかったら便利だよなーってことでそういうコマンドをつくった。
それを話した内容。
今も便利に活用している。

<script async class="speakerdeck-embed" data-id="78832e3017df013292f406657be3bf12" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* [LT駆動開発で hubot をつかったオフィスに誰がいるかわかるコマンドを作成した話をした - そんなこと覚えてない](http://blog.eiel.info/blog/2014/09/06/ltdd-07/)

### 戦闘力

GitHub戦闘力を提案してみた。
今年もっともPVが多いスライド。

<script async class="speakerdeck-embed" data-id="9f557cd01d560132ff4612198c64cd5d" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* [GitHub戦闘力を提案してみた - 座駆動LT大会 - そんなこと覚えてない](http://blog.eiel.info/blog/2014/09/13/github-scouter/)


### 電子書籍に挑戦した。その時に利用した技術のさわりを紹介

対象者設定に失敗したあのスライド。
OSC広島に展示するのに電子書籍を作成した時の話をした。
こういうのが流行してるよ感でキーワードを伝えるだけの目的だったけど速すぎた思い出が残っています。

<script async class="speakerdeck-embed" data-id="5010f1e023630132a9090a763f010e40" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>


* [すごいHirohsimaの本について、OSC2014とWTM71で紹介した - そんなこと覚えてない](http://blog.eiel.info/blog/2014/09/21/great-h-book-osc-2014-and-wtm71/)

### 内包表記(仮)

モナドの内包表記をしってからやろうと思ってたネタを放出した。
広島のITコミュニティの合同勉強会という立ち位置のイベントだったけど、もっといろんなコミュニティと協力したかった。
これも結構伸びた子である。

<script async class="speakerdeck-embed" data-id="7cb24810446c0132e04e4e24d1028d6d" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* [内包表記(仮) // Speaker Deck](https://speakerdeck.com/eiel/nei-bao-biao-ji-jia)


### 黒背景に青文字はやめろ

出オチと言われたアレ。
インターネット上にはあまり拡散されなかった子。
まあ中身があんまりないしね。

個人的にはまあまあ気に入っている。

<script async class="speakerdeck-embed" data-id="6bc3a81065840132d6705e4dc25c732c" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* [黒背景に青文字はやめろという話を合同勉強会in大都会で話をした。 - そんなこと覚えてない](http://blog.eiel.info/blog/2014/12/14/gbdaitokai2014-winter-color/)

# まとめ

いっぱいセッションした。
しかし、やっぱり喋るのは苦手なのはあまり治らない。

ダメな部分は意識して挑戦していきたいと思う。

来年は控えめになると思います。たぶん。

# 関連

* [2014年を振り返る - GitHub編 - そんなこと覚えてない](http://blog.eiel.info/blog/2014/12/31/2014-github/)
* [2014年を振り返る - ブログ編 - そんなこと覚えてない](http://blog.eiel.info/blog/2014/12/31/2014-blog/)
