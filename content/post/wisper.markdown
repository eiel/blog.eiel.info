---

title: "Wisper 試した"
date: 2013-12-25T01:11:00+09:00
comments: true
categories: ["rails","wisper"]
---

[Wisper](https://github.com/krisleech/wisper) を試した。

Rails のプラグインで、依存性の管理や分離ができる。
ActiveRecord の callback だと依存の記述がモデル上になって、
Observer は本家から外れたような気がするし(うる覚え)、
ActiveSupport::Notification じゃ依存性がゆるすぎる。
でも、コントローラでかくにはちょっと処理が多すぎるし…って時に探してたらみつけたプラグイン。

やってることはどれも大差ない気がするんだけど、とりあえず、公式のサンプルコードをみてみる。

```ruby
class BidsController < ApplicationController
  def new
    @bid = Bid.new
  end

  def create
    @bid = Bid.new(params[:bid])

    @bid.subscribe(PusherListener.new)
    @bid.subscribe(ActivityListener.new)
    @bid.subscribe(StatisticsListener.new)

    @bid.on(:create_bid_successful) { |bid| redirect_to bid }
    @bid.on(:create_bid_failed)     { |bid| render :action => :new }

    @bid.commit
  end
end
```

create_bid_successful が発生したときはリダイレクトして、create_bid_failed が発生した時は new を render するというのをアクションでかける。
ifの分岐がなくて、縦にコードが並んで複雑な分岐をする場合は読みやすい。
また、Listener を登録することで機能追加ができる。
create_bid_successful イベントがおきたときは、`PusherLister#create_bid_successful` や `ActivityListener#create_bid_successful`、`StatisticsListener#create_bid_successful` なんかも呼ばれる。
Set で保存されてるようなので順番は保証されない

メインの処理をモデルで集中できて少し脇道に逸れるようなものは Listener を書いて `subscribe` していけばいいという感じである。

また、 [wisper-async](https://github.com/krisleech/wisper-async) っていうのがあって、脇道に逸れるものは非同期に実行したりできそう。

イベントを起こすには `Wisper::Publisher` を `include` しておいて `publish` を呼ぶだけです。引数を渡したいときは一緒に渡します。

```ruby
publish(:create_bid_successful, self)
```

単体テストもしやすかったです。

個人的にはオブジェクトを subscribe すると反応しないイベントがある場合は警告してくれたら嬉しいなぁ、と思いながらちらちら見ている。
メソッド名を間違えるとちょっとめんどくさい。

エラーが発生したとき、わかりにくかったのもちょっと欠点か。

規模が大きいアプリケーションには使えそうな感じがしました。
