---

title: "Github の Feed が溜りすぎてやばい。ついでに気になるのをメモっとく - すごい広島 010"
date: 2013-07-24T19:18:00+09:00
comments: true
categories: ["Github"]
---

 この記事は [すごい広島 #010](http://great-h.github.io/events/event-010.html) でやったこと。

GitHub の News Feed が読めてない、ヤバイ。6月17日から溜まってる!
この時間を使って眺めて気になったのを簡単にまとめる。
ようするに、頭の中を垂れ流すだけの記事です。

### One Hundred Ideas for Computing

* [One Hundred Ideas for Computing](https://github.com/samsquire/ideas)

「クラウドコンピューティングでやりたい100のこと」といったころなのでしょうか。本当に100個あってびびった。ネタがないときのインプットにしたい。

あと、すでに存在しているものや、似ているものにリンクが貼られていて面白いです。100個あるとタイトルを読むだけで精一杯でした。

### http_configuration

* [http_configuration](https://github.com/bdurand/http_configuration)

ruby の gem。Rails の plugin としても利用可能らしい。

NET::HTTP のグローバル設定が可能らしい。タイムアウトする時間やプロキシを登録しておけば、自動で引き継ぐ感じなのでしょうか。試してないから知らない。

```ruby
config = Net::HTTP::Configuration.new(:proxy => :environment, :read_timeout => 10, :open_timeout => 5)
config.apply(:read_timeout => 5) do
  Net::HTTP.get('http://example.com/')
end
```

### pjaxメモ

* [pjaxメモ](https://gist.github.com/furu/6006109)

ブラウザの履歴操作のメモのようだ。整然としていてナイスである。
まだまだ充実していくんだろうか。

### glim

* [GRUB2 Live ISO Multiboot - glim](https://github.com/thias/glim)

いろんなOS Live CD をブートできる USB メモリや CD を作成できるみたい。
CDイメージを用意する必要があるのか気になったけど、そこまでは見ませんでした。

### boris

* [boris](https://github.com/d11wtq/boris)

PHP の REPLのようだ。Read Eval Print Loop。対話環境というやつか。

### reform example

* [reform example](https://github.com/gogogarrett/reform_example/tree/master/app)

[reform](https://github.com/apotonick/reform) という フォームを作成するための gem の利用例のようです。
reform を知らないので、そっちも見てみた。

Rails でも利用できるし単体でも利用できるみたい。
ネストしたモデルが扱いやすいような感じなのでしょうか。わかりません。

サンプルの中に

* app/services
* app/forms

というディレクトリがあって、モデルにはバリデーションが全く書かれていないのが興味深いです。

### I18n for Javascirpt

* [i18n-js](https://github.com/fnando/i18n-js)

Rails の i18n を JavaScirpt 上でも利用するためのライブラリのようです。
JavaScript 側でやることが増えてきてるので気になるところ。

### Bower

* [Bower](https://github.com/bower/bower)

最近よくみかける JavaScirpt の Bundler 的なやつ。
Rails でも使う人が増えてるらしいけど、私はまだ使えてない。

### Rspec Style Guide

* [Rspec Style Guide](https://github.com/howaboutwe/rspec-style-guide)

あとで 10回 読む。中はまだ見てない。watch する。しばらく毎日読む。

### Rspec Best Practices

* [Rspec Best Practices](https://github.com/crible/rspec-best-practices)

あとで 10回 読む。中はまだ見てない。watch する。
11ヶ月まえなので最近の傾向が取り入れられてるのが気になる。

### KawaiiEmailAddress

* [KawaiiEmailAddress](https://github.com/esminc/kawaii_email_address)

Railsで docomoやらauやらの Email アドレスのバリデーションができるようだ。

### byebug

* [byebug](https://github.com/deivid-rodriguez/byebug)

バグとさようなら。Ruby のデバッガのようです。
特徴は…読む元気がなかった。pryが起動できるとかは見えた。
気になる。

### PhantomFlow

* [PhantomFLow](https://github.com/Huddle/PhantomFlow)

すごく気になる。ウェブユーザインターフェースのテストをするために、フローを視覚化してるツールのように見える。
シナリオを分岐させられる感じなのでしょうか。気になります。

### CSSeCSS

* [CSSCSS](https://github.com/zmoazeni/csscss)

CSSを解析して、重複してるコードを見つけてくれる模様。

### default_value_for

* [default_value_for](https://github.com/FooBarWidget/default_value_for)

rails のプラグイン。モデルのデフォルト値を簡単に設定できるようです。

### まとめ

50件ぐらいは消化した。まだ140件ぐらいある。もうちょっと消化したかった。

メモしながらなのでペースは落ちますが「メモを残すぐらいは確認しないといけない」と意識できます。
読んだ気にならず、しっかり読めて良いかもしれません。
実際に試す時間をどう取るのかというのが悩みどころです。
