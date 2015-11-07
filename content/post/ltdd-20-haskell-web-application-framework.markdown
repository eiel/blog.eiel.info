---
date: 2015-11-07T23:27:49+09:00
title: "HaskellのWebApplicationFrameworkを試してみるという話をした - #LT駆動 20"
---

[LT駆動開発20](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA20)で「HaskellのWeb Application Frameworkを試してみる」という話をした。

<script async class="speakerdeck-embed" data-id="88440dd1822743159eed085c25fe1850" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

試したのはいわゆるController的な部分になる。最近Webというのはヘキサゴナルアーキテクチャのアダプタにみえてしまう病にかかっています。

話しが逸れました。
前々からHaskellでWeb APIをかきたかったのですが、[小噺マスターさん](https://twitter.com/uadachi)と小噺をしている際に「Haskellのフレームワークってなんですかねー？」という話が出たのを機会にちょっと調べた。
気軽にデプロイできることもわかったし、これを機会にざくっと試してみたという感じです。

[scotty](https://github.com/scotty-web/scotty)に改良を加えたのが[Spock](https://www.spock.li/)って感じの印象をうけました。ふたつとも手軽に使えました。
[Yesod](http://www.yesodweb.com/)は別格で難しく感じましたが、機能的にはすごいなーって思いました。
3つともコードはシンプル。

しばらくは、Spockで遊びつつYesodを勉強しようかと思っております。

* [ソースコード](https://github.com/eiel/haskell-webframework-sample)

### 関連

* [stackを使ってhaskellで最小のWeb Applicationしてみる - Qiita](http://qiita.com/eielh/items/09b9f1c21f7d16e09ede)
* [herokuにhaskellのウェブアプリをdocker:releaseをつかってデプロイしてみる - Qiita](http://qiita.com/eielh/items/e52aeee1419ba611a84d)
