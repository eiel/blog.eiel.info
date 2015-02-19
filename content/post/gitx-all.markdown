---

title: "gitx --all"
date: 2012-06-13T18:00:00+09:00
comments: true
tags: [gitx,git]
---
skypeでgitの説明していたときに `gitxa` って何？と問われた。単なる`gitx --all` のエイリアスです。

```
alias gitxa="gitx -all"
```

# Gitx

Mac用のgitクライアントツールに[Gitx](http://gitx.frim.nl/)というものがあるのですが、コミットの機能なども備えていますが、私は`gitk`コマンドの代替として利用しています。

![](http://media.tumblr.com/tumblr_m5jh47eL8d1qk2e7q.png)

ただし、Gitxをインストールしただけでは `gitx` コマンドは使用できません。Gitxのメニュー内のEnabble Terminal Usaeg... をクリックすることで利用できるようになります。

![](http://media.tumblr.com/tumblr_m5jhgcOZxD1qk2e7q.jpg)

`gitx`を引数になにもなしで、呼びだした場合、checkout している branchを表示します。`--all`を使用すると`all branches`を選択した状態になります。
他にもときどき便利なのは `gitx ブランチ名` や `gitx -- ファイル名` などがあります。gitkを使う場合はさらにいろいろできます。詳細は `man gitk` にて。
