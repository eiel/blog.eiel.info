---

title: "Mac で使える git mergetool をいろいろ試してみる - xxdiff"
date: 2013-07-03T21:27:00+09:00
comments: true
categories: ["git","xxdiff","mac"]
---

[Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/) の続きです。

つづいて [xxdiff](http://furius.ca/xxdiff/) を紹介したいと思います。

Ot で実装されているっぽいです。
マージ結果が常に表示されないため扱いにくいです。
また、Retina に対応していないみたいで、文字が読めませんでした。
あと、日本語が文字化けしました。

設定は不要で、

```
git mergetool -t xxdiff
```

とすると起動できます。

また、常に xxdiff を利用したい場合は

```
git config --global merge.tool xxdiff
```

としておけばよいです。

![xxdiffの画面](/images/xxdiff.png)

画面左に現在のブランチのファイル、画面右にマージするブランチのファイルの内容が表示されます。

キーボードショーットカットは充実しているようですが、マージ結果をみる方法がわかりませんでした。

日本語はでないし、マージ結果の出し方がわからないということで利用は難しそうでした。

# 関連

* [Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/)
