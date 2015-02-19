---

title: "Mac で使える git mergetool をいろいろ試してみる - OpenDiff"
date: 2013-06-26T21:10:00+09:00
comments: true
tags: ["git","mac","opendiff"]
---

[Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/) の続きです。

Xcode に標準添付されていた opendiff を紹介します。
[Source Tree](http://www.sourcetreeapp.com/) にもついてきていましたが、挙動が微妙に違いました。

設定は不要で

```
git mergetool -t opendiff
```
と入力すると利用できます。

`git mergetool` だけで起動できるようにしたい場合は

```
git config --global merge.tool opendiff
```

としておくとよいです。

日本語がまざっていると、  `file are not ascii` と メッセージ がでますが、 proceed anyway をクリックすると問題なく動きます。

opendiff で起動しますが FileMerge というアプリケーションのようです。

![OpenDiff の画面](/images/opendiff.png)

左側に checkout しているブランチのファイル、
右側に merge しようとするブランチのファイルが表示されます。
下側に マージ結果が表示されます。

デフォルトで 右側のものが選択されていました。

競合箇所をクリックして、画面右下の actions から選択することで、どちらの変更を使うか、両方を使うかなどが選べます。

競合箇所は真ん中の矢印がでている部分ではないと機能しないようでした。

キーボードで移動したい場合は `command + d` で次へ進み、 `command + shift + d` で前へ戻ります。

下部の部分は 編集可能で、修正を加えることもできます。

修正が修正したら、閉じてコマンラインに

```
Was the merge successful? [y/n]
```

と表示されているので、`y` を入力して Enter です。なにか間違えた場合は、`n`にすることでなかったことにできます。
他にも競合するファイルがあれば、次のファイルがでてきます。

マージする前のファイルが .ファイル名に `.orig` という文字が付加されて残っています。

日本語が使えているので、設定するのが面倒であればこれでもいいかなと思いますが、Action の選択がめんどくさかったです。

# 関連

* [Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/)
