---

title: "Rspecマッチャー rspec-html-matchersを試してみてる"
date: 2012-06-22T18:04:00+09:00
comments: true
categories: [rspec,ruby,rails,rspec-html-matchers]
---
Ruby on Railsで ViewやHelperの Specを書く際に利用するマッチャーに良いのがないか探してます。現在のRspecはcontainぐらいしかないので、細かくチェックしたい場合は若干使いづらいです。というわけで、[rspec-html-matchers](https://github.com/kucaahbe/rspec-html-matchers)を試しています。

以前は [rspec-tag_matchers](https://github.com/dcuddeback/rspec-tag_matchers) を使用していたのですが、出力がちょっとイマイチでした。

[Ruby Tools](www.ruby-toolbox.com)でざらざらと探した結果、[rspec-html-matchers](https://github.com/kucaahbe/rspec-html-matchers)を試してみることにしました。

Form用のマッチャーがいろいろあったり、内部に存在するタグをチェックしたりできるのが嬉しいですね。capybaraの`have_css`はsubject側で find(selector)しておく必要があるので、ややめんどくさいです。

いまのところの不満点は

* Hashで渡していくのがちょっと格好悪い
* 正規表現での属性チェックができなかった
* 暗黙的なsubjectを使用する場合、ブロックがあると不具合がでる

3番目なんですが、have_tag マッチャーにブロックを渡し場合 shouldメソッドのレシーバをかかないと、ブロック内へと処理が流れないようです。


```ruby
subject { render }
it do
  should have_tag("a") do
    # このブロック処理が走らない
    with_tag("b")
  end
end
```


のように書いてしまうと `with_tag("b")` の部分が動作しません。3行目を明示的に `subject.should`とすると動いてくれました。rspecの問題なのか、rspec-html-matchersの問題なのか切りわけが難しいのでとりあえず、我慢することにしました。
ブロックを渡さない場合は大丈夫です。

他は良好に使えています。
View Specの良い例が欲しいです。
