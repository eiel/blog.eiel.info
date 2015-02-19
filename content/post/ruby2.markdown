---

title: "ruby-2.0.0をビルドしてみた on rbenv"
date: 2012-11-07T00:48:00+09:00
comments: true
categories: ["ruby"]
---

Ruby 2.0.0 preview1 の話題をちらほら見かけますし、 heroku さんが対応したらしいので遊びのプロジェクトで使おうと思いまして、buildしてみました。

私は `rbenv` を `git clone` でインストールしていて、ruby-buildも `git clone` しています。
この場合以下の操作でビルドできます。


```
$ cd ~/.rbenv
$ git pull
$ cd ~/.rbenv/plugins/ruby-build
$ git pull
$ rbenv install 2.0.0-preview1
$ rbenv global 2.0.0-preview1
$ ruby -v
ruby 2.0.0dev (2012-11-01 trunk 37411) [x86_64-darwin12.2.0]
```

git だと更新も楽チンですね。rails プロジェクトでも試してみたり、新機能を試してみたりしたいと思います。

新機能については 下記のサイトとかにちょろちょろあるみたいです。

* [http://el.jibun.atmarkit.co.jp/rails/2012/11/ruby-20-8256.html](http://el.jibun.atmarkit.co.jp/rails/2012/11/ruby-20-8256.html)
* [https://speakerdeck.com/a_matsuda/ruby-2-dot-0-on-rails](https://speakerdeck.com/a_matsuda/ruby-2-dot-0-on-rails)

自分で試したら記事にしたいと思います。
