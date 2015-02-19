---

title: "Gentoo Prefix で python3.2 がないって言われてインストールできない"
date: 2013-03-06T14:08:00+09:00
comments: true
categories: ["gentoo"]
---

いつのまにやら Gentoo Prefix が python 3.2 を入れようとするのだけど、 python 3.2 の ebuild はありません。
`emerge -uDNt world` とかで更新しようとしたとき python 関連のものがあると実行できない。以下のようなメッセージがでます。

```
emerge: there are no ebuilds to satisfy "dev-lang/python:3.2".
```

`python 3.3 はいってるし、別にいらないよね。` ということで USE フラグに `-python_targets_python3_2` を追加したらうまくいった。

TARGET_PYTHONを調整したくなるのですが、これは 上記の USE フラグに展開される。
