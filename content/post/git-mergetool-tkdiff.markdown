---

title: "Mac で使える git mergetool をいろいろ試してみる - tkdiff"
date: 2013-07-03T21:07:00+09:00
comments: true
categories: ["git","tkdiff","mac"]
---

[Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/) の続きです。

[tkdiff](http://tkdiff.sourceforge.net/) を紹介します。
名前のとおりツールキットは tk なのでマルチプラットフォームのアプリケーションです。

マージ結果が同時に表示されないので非常に使いにくいです。
最近は Mac で tk のアプリケーションの起動速度が早くなってるのに少し残念です。
ツールキットの都合、表示も他に比べると綺麗ではありません。

設定は不要で、

```
git mergetool -t tkdiff
```

とすると起動できます。

また、常に tkdiff を利用したい場合は

```
git config --global merge.tool tkdiff
```

としておけばよいです。

![tkdiff の画面](/images/tkdiff.png)

画面左に現在のブランチのファイル、画面右にマージするブランチのファイルの内容が表示されます。

マージされる内容を表示するには、画面上部の緑の部分をクリックするとみることができます。
左や右やじるしをクリックすることで、変更を選択できます。

ツールキットの都合、使いにくいです。常用には耐えそうにありまんでした。

# 関連

* [Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/)
