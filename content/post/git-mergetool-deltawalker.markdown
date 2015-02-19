---

title: "Mac で使える git mergetool をいろいろ試してみる - DeltaWalker"
date: 2013-07-03T19:16:00+09:00
comments: true
categories: ["git","deltawalker"]
---

[Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/) の続きです。

[DeltaWalker](http://www.deltopia.com/) を紹介します。

DeltaWalker 49ドル の有料アプリケーションで、見た感じ Eclipse と同じツールキットのようで、マルチプラットフォームを実現しているようです。
しかし、購入する際はOSを指定するので、OSごとにライセンスがいるのでしょうか。試用ができるので試してみました。

有料ツールの中では値段が安く、日本語を使用していても問題はおきてないです。

設定するには `/Applications` にインストールした場合は

```ruby
git config --global --add mergetool.dw.cmd '"/Applications/DeltaWalker.app/Contents/MacOS/git-merge" "$LOCAL" "$REMOTE" "$BASE" "$MERGED"'
```

で設定できます。

起動するには

```
git mergetool -t dw
```

とするとできます。

デフォルトで起動するツールに設定したい場合は

```
git config --global merge.tool dw
```

で、設定することができます。

![deltawalker の画面](/images/deltawalker.png)

画面左にもとのブランチのファイル、画面右にマージするブランチのファイルが表示されます。中央にマージ結果が表示されています。

マージ方式は emacs の ediffと同じように、先にベースしたいほうを選びます。

そうすると細かいコンフリクト箇所が表示されるので、どちらを選択するか選ぶことができます
。
左を選ぶか、右を選ぶかを選択するための状態にもっていくのが少し扱いづらかったです。
なぜだかわからないですが、左しか選べない状態になったりしました。マウスクリックですることで直りました。

コンフリクトの修正後は、ウィンドウ閉じれば次へ勧みます。
他にコンフリクトするファイルがあれば続けて表示されます。

修正が終了するとコンフリクトしたときのファイルは `.orig` が末尾について保存されます。

気になる点は Java で実行するため、起動が少し遅いです。

# 関連

* [Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/)
