---

title: "rackup で ruboty"
date: 2014-06-08T10:31:00+09:00
comments: true
categories: ["ネタ","ruboty"]
---

[ruboty](http://r7kamura.hatenablog.com/entry/2014/05/31/190240) を heroku で動かすついでに web アプリケーションとして扱いたい。
その場しのぎで書いたコードを紹介する。

`config.ru` の中で別スレッドを起動して Ruboty#run を走らせた。

```ruby
# Rackアプリになるように call メソッドを実装
module Ruboty::Web
  def call(env)
    [200, {'Content-type' => 'text/html'}, ['hello, world']]
  end
end
Ruboty::Robot.include(Ruboty::Web)

Thread.new do
  robot.run   # bot起動
end

run robot  # rack アプリを構築
```

heroku では 1X dynos でもスレッドが 256 個ぐらい起動できるっぽいしきっと大丈夫だろう。いや、わからんけど。

* 参考: [Limits | Heroku Dev Center](https://devcenter.heroku.com/articles/limits)

シグナル処理も追加したほうが良い気がするけど、とりあえず気にしない。

そのうちウェブアプリを構築しやすくする方法を考えたい。

### 関連

* [hubot で起動しているウェブサーバを経由して idobata へ投稿してみる](http://blog.eiel.info/blog/2014/05/27/message-send-from-hubot-httpd-to-idobata/)
