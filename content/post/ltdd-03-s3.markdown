---

title: "LT駆動開発03で「S3にスライドを保存することにした」という発表者をしてきた。"
date: 2014-05-03T23:29:00+09:00
comments: true
categories: ["LT駆動","S3","Haskell"]
---

[LT駆動開発03](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA03) でLTをしてきました。

<script async class="speakerdeck-embed" data-id="0917d000b4c40131d8ee7625813d8974" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

最近、本当にSSDの容量不足が深刻で仮想マシンなんて作った日にはさっさと用事を済ませて消さないとやばい状態が続いています。

そんなわけで、滅多に使うことのないけど、残しておいたら役に立ちそうなファイルはS3に保存してみることにしました。
それだけだとつまらないので、ついでに一般公開しました。

index.html を作るのに手動で作るのはめんどくさいので、
S3のバケットに保存しているオブジェクトの一覧をAPI経由で取得して、この情報を元に index.html を作成して、アップロードして、静的サイトとして公開しています。

1. 一覧の取得
1. index.html の作成
1. index.html のアップロード

を自動化しています。

実際のページとソースコードはこちらに。

* [http://keynotes.eiel.info/](http://keynotes.eiel.info/)
* [GitHub - eiel/keynotes-eiel](https://github.com/eiel/keynotes-eiel)

S3を使った静的サイトの運用もちょっと試してみたかったので良い機会でした。まだ、調査が必要そうですけども。

自動化はしていない部分であるスライドのアップロードには [s3cmd](http://s3tools.org/s3cmd) を利用しています。

後半はHaskellの話です。
HaskellのAWSのAPIを叩くには [aws](https://hackage.haskell.org/package/aws) ライブラリを使用しました。他にも [aws-sdk](https://hackage.haskell.org/package/aws-sdk) というライブラリもあるようですが、対応しているサービスが違っているようでした。

元々 Ruby を使ってつくってたのですが、なんとなく理由もなく Haskell でやりたくなってHaskellでやってみましたが、意外と簡単にできました。
Haskell 良いですね。

あとレコード型が実に使いこなせてないことに気づいたので勉強してこようと思いました。

まだ、 keynotes.eiel.info に特化しているのでそのうち汎用性を上げたいと思います。

スライドですが、[azusa テンプレート](http://memo.sanographix.net/post/82160791768)をかなり参考にさせていただきました。
というか、色は少し自分好みに変えただけですね。
赤はもうちょっと明るい色を使うほうがいいなって思う反省点がありました。

そんなわけで、今日はこの辺で。

### 関連リンク

* [LT駆動開発という勉強会をはじめるよ - そんなこと覚えてない](http://blog.eiel.info/blog/2014/02/19/start-ltdd/)
