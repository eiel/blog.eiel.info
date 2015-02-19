---

title: "zeus test で スペックを実行すると 2度実行されてしまう"
date: 2012-12-27T18:05:00+09:00
comments: true
tags: ["rspec","zeus","rails"]
---

`zeus test` で spec を走らせるとなぜかスペックが2度実行されるようになっていた。
`spec/spec_helper.rb` 内の `require 'rspec/autorun'` を削除すると治るようです。

`spork` で実行してみたり、 `rake spec` したりもしてみたけど、消したから起きている問題は今のところないです。

### 追記

`zeus rake spec` したときに DB がリセットされてなくて上手く動いてないことがわかった。全件まわしたい場合は `rake spec` を使用してたので気がつかなかった。

### 参考
[https://github.com/burke/zeus/issues/180](https://github.com/burke/zeus/issues/180)
