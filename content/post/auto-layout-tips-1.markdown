---

title: "AutoLayout TIPS - 真ん中に固定幅のスペース"
date: 2013-05-19T12:04:00+09:00
comments: true
tags: [ios,autolayout]
---

AutoLayout になかなか慣れません。
そうは言っても使わなければ、身につかない。
<del>というか久しぶにりiOSのコード書いてるだけな気がする</del>

今回挑戦したのはふたつのViewの間に固定幅をスペースをつくりたい。
具体的には以下の感じ。

![autolayout](/images/autolayout-fixed-center.png)

書いた VisualFormatLanguage はこんな感じ。

```
|-[_leftView]-40-[_rightView]-|
[_leftView(==_rightView)]

V:|-[_leftView]-|
V:|-[_rightView]-|
```

以下のように書いてもよい。

```
|-[_leftView(==_rightView)]-40-[_rightView]-|

V:|-[_leftView]-|
V:|-[_rightView]-|
```

やってみると簡単。

プログラムで気軽にレイアウトできる。

[具体的なソースコードは Github に アップしています。](https://github.com/eiel/AutoLayoutTip1)

[主な処理はこの辺にあります。](https://github.com/eiel/AutoLayoutTip1/blob/master/AutolayoutTip1/ALViewController.m#L44-L66)

### 簡単に解説

頭に `V:` がついているのは 縦方向に対する設定です。

`|-` の部分は OS 標準の幅になります。ぴったりつけたいなら、`|-0-` とします。

縦方向の左側 だけやってみます。
```
V:|-0-[_leftView]-0-|
```

### まとめ

なれるまで発想のセンスがいる気がします。

論理的な手順で、作りたいレイアウトをするのはまだまだまだ説明できそうにないです。
