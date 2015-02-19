---

title: "ReVIEW を試してみる"
date: 2013-08-14T18:47:00+09:00
comments: true
tags: ["review"]
---

毎週のアレ、[すごい広島 #013](http://great-h.github.io/events/event-013.html)で試したこと。

[ReVIEW](https://github.com/kmuto/review) という軽量マークアップ言語に属すると思われる電子書籍を作成するのを主眼に置いてるものみたいです。先週TLでやたらみかけました。

というわけで、[QuicqStart](https://github.com/kmuto/review/blob/master/doc/quickstart.rdoc) を参考に試してみました。

### インストール

まずは、gem でインストールしました。

```
gem install review
```

Ruby使いにはとても簡単。

### とりあえず epub をつくってみる

epub 作るのに作成したファイルは以下の通り。

* sample.re
* sample.yaml
* CHAPS

最後に

```
review-epubmaker sample.yaml
```

で、epub が作成されました。

それぞれのファイルの中身ですが、`sample.re` が、

```
= はじめてのReVIEW

//lead{
「Hello, ReVIEW.」
//}
```

`sample.yml` は [https://raw.github.com/kmuto/review/master/doc/sample.yaml](https://raw.github.com/kmuto/review/master/doc/sample.yaml) からダウンロードしました。

最後に `CHAPS` が、

```
sample.re
```

です。

そして、iPhone で動作確認してみました。
iPhone に送るには iTuens からの同期するのが良さそうですが、メールで送信しました。
Message で送るとなぜだか認識しませんでした。Dropbox もありだと思います。

PDF作成するためには TeX環境がいるのでまた別の機会にします。

### reファイル

内容を書くファイルみたいです。RDに近い文法です。

詳しい文法は [ReVIEW フォーット](https://github.com/kmuto/review/blob/master/doc/format.rdoc) を参考にしてください。

ちらっとみてみるると、コラム専用のものがあって、書籍用って感じがして良いですね。RDに近いと書きましたが、むしろ LaTeX に近いようにも思います。

### CHAPS

`CHAPS` ファイルは章を記述するファイルで、1章につき1ファイルになるようです。似たようなファイル`PREDEF`、 `POSTDEF` があってこれは、本文ではない前書きの部分や後書きの部分を定義するようです。
前付け、後付けというものに該当するようですが、専門家でないのでよくわかりません。

### sample.yml

書籍の情報を記述するものみたいです。
コマンドの引数に渡すので好きなファイル名にできます。

### まとめ

[Wiki](https://github.com/kmuto/review/wiki) にもいろいろ情報があります。
ReVIEWの[Emacsのメジャーモード](https://github.com/kmuto/review-el)もあるみたいです。
折角なので、[el-get の レシピ](https://gist.github.com/eiel/6230068) を適当に書いてみました。
el-get-recipe-path にディレクトリを追加して `reiview-mode.rcp` とい名前で保存して、`(el-get 'sync 'review-mode)` を評価すれば設定できます。デフォルトのカラー設定が背景が暗い場合つらいので、 face を変更しないと使いづらそうでした。

せっかく設定したのでなにか電子書籍を書いてみたいですね。何かないかなー。
