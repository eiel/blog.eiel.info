---

title: "Mac で使える git mergetool をいろいろ試してみる - Araxis"
date: 2013-07-10T01:54:00+09:00
comments: true
tags: ["git","mac","araxis"]
---

[Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/) の続きです。

[Araxis Merge](http://www.araxis.com/merge_mac/index.html) を紹介します。

Cocoaアプリで表現も操作性も良いです。
だけど、値段を考えるとすこし残念な感じでした。

値段は 120ドルと [Kaleidscope](/blog/2013/06/29/git-mergetool-kaleidoscope/)の2倍近い値段です。
ちょっと高い。

マージモードなのに3カラムではなく、2カラムでどのようにマージされるのかわかりにくかったり、キーボードショートカット が fn + ↓ とかになっていて効かなかったりと、そのあたりが残念でした。

設定方法ですが、git にあらかじめ設定がもりこまれているため、インストールして、環境変数PATHを設定してやれば利用できます。

```
export PATH=/Applications/Araxis\ Merge.app/Contents/Utilities:$PATH
```

ここもまた残念で、compare というコマンドで起動するようですが、 他にもcompareというコマンドがあると、これより先にコマンドがみつかるように PATH を設定する必要があります。

公式の設定だと `PATH=$PATH:/Applications/Araxis\ Merge.app/Contents/Utilities`とかかれていてハマりました。

あとは

```
git mergetool -t araxis
```

と、実行すると起動することができます。

常に Araxis を利用したい場合は

```
git config --global merge.tool araxis
```

としておくと `-t  araxis` を省略できます。

![araxis の画面](/images/araxis.png)

画面左が現在のファイル内容で、画面右がマージしようとするブランチのファイルの内容になります。保存して画面を閉じれば左側の内容に決定したことになります。
左側がデフォルトで選択されていて、右側を採用したい場合は選んでいくという使い方になります。

TimeMachine との連携などもでき多機能で優秀なのですが、`git mergetool`として使うにはちょっと悩む感じでした。
有料だけあって、無料ツールよりはずっと使いやすいです。

# 関連

* [Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/)
