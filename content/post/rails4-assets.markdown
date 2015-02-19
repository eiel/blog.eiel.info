---

title: "rails4 で Asset の生成したいファイルの追加がうまいこといかなかった。"
date: 2013-05-04T00:44:00+09:00
comments: true
categories: [rails]
---

Rails4 を試していた。
1ページだけ全く違うデザインがあって js とか css を application.js とか application.css とは別に生成したかった。

ということで `config/environments/production.rb' に

```ruby
config.assets.precompile += %w( hoge.js )
```

と書きました。

なぜか生成されない。Rails3 では間違いなく生成される。

調べてみたところ `config/application.rb` に書けば動くことがわかった。
それだけ。
