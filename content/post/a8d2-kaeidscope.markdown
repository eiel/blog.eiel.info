---

title: "LT駆動開発で UIDynamics を利用した万華鏡アプリを紹介した"
date: 2014-08-02T12:00:00+09:00
comments: true
tags: ["LT駆動","swift","iOS","A8D"]
---

[A8D:2](https://www.facebook.com/events/321060364724219/) というイベントが2014年8月3日に行なわれます。明日じゃねーか。

そんなことはさておいて、[Augment8](http://augment8.org/)というグループがあります。
このグループは年に1回、普段作っているものをみんなに体験してもらう場をつくろうというイベントがあるらしく、Augment8 Day を省略して A8D というイベントをしているそうです。

デジタルなガジェットを一般の人に体験していただくイベントだ。

そのイベントに参加することになったので、このために作成したアプリを告知を兼ねて[LT駆動開発06](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA06)で紹介をしました。

というわけでまずスライド。

<script async class="speakerdeck-embed" data-id="4e0e1b60fb610131b3ca3a8923229263" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

つづいて、スライド内で利用している動画。

万華鏡のサンプル1

<iframe width="420" height="315" src="//www.youtube.com/embed/sMJtJsOGcPg" frameborder="0" allowfullscreen></iframe>

万華鏡のサンプル2

<iframe width="420" height="315" src="//www.youtube.com/embed/GXqg2EEn-d4" frameborder="0" allowfullscreen></iframe>

UIDynamicsのサンプル

<iframe width="420" height="315" src="//www.youtube.com/embed/4AIo5x1DoZo" frameborder="0" allowfullscreen></iframe>

iOS7 からUIKitで物理エンジンが搭載されていて気軽に使えるようになったらしい。
まだ試してなかったのだけど、普通の人にも楽しめそうなものとして万華鏡をつくってみた。
iOSのCoreMotionを使いiOS端末を操作して画面に映る万華鏡が変化するという寸法です。

普段、iOSアプリをつくってるわけではないのでそんなに凝ったことはしていません。
万華鏡といえば三角なのですが四角のものもあるそうで、手抜きで四角の万華鏡になっています。

Apple Swift で作成しているためアプリとして申請するのもできないし、体験できるようにソースコードを公開しておきますね。まだ勉強中でソースコードが汚いですね、わかります。

* [Augment8/kaleidoscope · GitHub](https://github.com/Augment8/kaleidoscope)

サブディスプレイに対応しているので、プロジェクタに写したり Apple TV で画面に写したりできます。

当日は丸いものにプロジェクトションマップもどきをしたりする予定です。
操作用のiOS端末のために**拡張パーツ**を用意しています。デザイン担当の[あきさん](https://twitter.com/akigonn)が作成しています。これを装着すると…つづきは当日のA8D:2にて体験してください。

デジタルならではなところは動的に反射している数が増えたり、中身が変化したり、マスクが変化したりと用意してみました。
それなりに面白くなったでしょうか。
魅せるためのスキル不足なため、本当に楽しめるものになったのかよくわからないので、みんなの反応が気になります。

今回のLT駆動における初挑戦は「動画をつかってみる」でした。
スライドを公開する際に動画は別のところにアップロードしないといけないのがすこしつらい。

Swiftのコードを書いていて思ったことは、

* ブロック構文がなかなか書きなれない
* 変数宣言で型から書きそうになる
* なぜか`［`を入力してしまう
* なるべつ let で済ませたい病
* selector 補完効かない。つらい。(文字列で渡すから当たり前)
* ヘッダファイルいらないヒャッハー
* CGFloat が絡む数値計算なんかよくわからない(すぐ型エラーに阻まれる)

などなどでしょうか。(咄嗟に思いついたものだけ書いた)

UIDynamics で ビヘイビアのインスタンスを節約しようとするとランタイムエラーではまったりしたのも良い思い出です。アニメータごとにインスタンスをつくりましょう。

Viewを触れるようにしてみたけど、移動したViewは元の位置にもどるという残念な結果にもなりました。

最後になりますが、調整するのに[BAUHAUS](https://bauhaus-web.jp/)の[上原さん](https://twitter.com/uehaso)や[ファナフェクト](http://funaffect.jp/)の方々にアドバイスをちょろっともらったりしました。さんきゅーです。
