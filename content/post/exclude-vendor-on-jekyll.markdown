---

title: "Jekyll 使うときは exclude: vendor しとけって話らしい。"
date: 2014-01-22T02:28:00+09:00
comments: true
tags: ["ruby","jekyll","github"]
---

素の[Jekyll](http://jekyllrb.com/) なんて使う人はあまりいないと思うけど一応書いておこう。

[以前から jekyll build が失敗するっていう話をしてる人がいて](https://github.com/great-h/great-h.github.io/issues/586)自分の環境じゃ、おきてなかったんだけど、`bundle install --path vendor/bundle` してるのが原因だったらしい。

`_config.yml に exclude: ['vendor']` するのがよいでしょう。

ついでに以下のような感じにした。

```
exclude: ['Gemfile','Gemfile.lock','Rakefile','vendor']
```

* [具体的なコミットはこちら](https://github.com/great-h/great-h.github.io/commit/8c99dc2d0ae37289ce65270587636f3da7447366)

素のJekyllから拡張してる場合は注意。
[middleman](http://middlemanapp.com/) などなどを使うことをおすすめしとこう。

自分が発見したネタじゃないけど記録しておいた。
