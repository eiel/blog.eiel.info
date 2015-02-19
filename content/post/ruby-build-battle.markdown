---

title: "ruby-build の プルリクエスト バトル"
date: 2013-02-24T23:14:00+09:00
comments: true
tags: ["ネタ","github","ruby"]
---

ネタです。

最近もろもろな事情で Ruby がリリースされることが多かったですが、ruby-build の更新を待っていた人はどれくらいいるでしょうか。

みんな待ちきれなくて自分で ruby-build のレシピを書いたんではないでしょうか？
そして「俺がと プル リクエストをおくるんだ!!」と燃えたのではないでしょうか？
これを Ruby のリリースがあるたびに発生する `ruby-bulild プルリクエストバトル`だと勝手に想像して楽しんでいます。こんばんは。

<del>僕の場合はだいたいなぜか `rbenv` のほうをみにいって、「まだ更新がないないなー」っておもってレシピをかくんですが、書いたあとに `ruby-build` だったと気づく馬鹿なことをしているだけだったりしますが</del>

今日も [Ruby 2.0 のリリース](http://www.ruby-lang.org/ja/news/2013/02/24/ruby-2-0-0-p0-is-released/) がありましたが、このプルリクエスト バトル の行方はどうなったのでしょうか。

* https://github.com/sstephenson/ruby-build/pull/299
* https://github.com/sstephenson/ruby-build/pull/301

同じ Issue を立てないように気をつけたいですね。

それと Ruby 20周年おめでとうございます。

## ついでにレシピの書き方

ネタだけで終わるのもあれなので。

`~/.rbenv` にインストールしている場合は `~/.rbenv/plugins/ruby-build/share/ruby-build/` にレシピが配置されています。

今回の 2.0 の場合は

```ruby
install_package "openssl-1.0.1e" "https://www.openssl.org/source/openssl-1.0.1e.tar.gz#66bf6f10f060d561929de96f9dfe5b8c" mac_openssl --if has_broken_mac_openssl
install_package "ruby-2.0.0-p0" "ftp://ftp.ruby-lang.org/pub/ruby/2.0/ruby-2.0.0-p0.tar.gz#50d307c4dc9297ae59952527be4e755d"
```

とか前のバージョンを参考にして書けばよいです。簡単ですね。

## なんでこんなことかいたのか

* なんで push してないんだー。って怒られたので
* Ruby 2.0 リリース & 20 周年 おめでとー。とかそういう記事書きたいじゃないですか
