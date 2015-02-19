---

title: "AbstractController を継承して遊ぶ"
date: 2013-09-04T19:03:00+09:00
comments: true
tags: ["Rails","ActionPack"]
---

[すごい広島 #16](http://great-h.github.io/events/event-016.html) で遊んだこと

Rails で AbstractContrller::Base を継承して **オレオレ コントローラ** を作りたいと思います。

どうしてこれをしたいかというと View です。
コントローラ名とアクションに対応した View がレンダリングできるのが魅力的です。

状況としてはメールを送るわけではないので ActionMailer 使えないし、すぐに画面に表示するわけではないので ActionController はちょっと…という感じなわけです。
どんな状況かというとDBに保存する長文を作成したい時です。

書いたビューはこんな感じ。

```ruby
こんにちは <%= @user %>さん

<%= goodbye_helper @user %>
```

ヘルパーも使えるようにしてみます。
ファイル名は `app/views/hoge_template/goro.text.erb` でコントローラ名が `hoge_template` アクション名 `goro` テンプレートエンジンは `erb` です。

利用には以下のように使います。
`rails c` の中でやったり、モデルの中で使えます。
もちろんコントローラ上でも。

```ruby
template = HogeTemplate.new
template.process(:goro)
template.render
```

renderの戻り値がテンプレートの出力結果になります。

最終的な出力目標は

```
こんにちは  おなまえさん

おなまえさん、おやすみ
```

を目指します。
`@uesr` には `おなまえ` が入っています。
`goobye_helper` はお別れのあいさつをしてくれるように実装します。

作成するコントローラ的な役割をする HogeTemplate はこのように書きたいはずです。

```ruby
class HogeTemplate < ActionTemplate
  def goro
    @user = "おなまえ"
  end
end
```

Helper は以下のように書きたいです。

```ruby
module ApplicationHelper
  def goodbye_helper(user)
    "#{user}さん、おやすみ"
  end
end
```

このようにできる ActionTemplate クラスを作るのが目的です。

その ActienTemplate クラスは下記で動作させることができました。

```ruby
class ActionTemplate < AbstractController::Base
  include AbstractController::Rendering
  include ActionController::Helpers

  helpers_path << 'app/helpers'
  helper :all

  view_paths << 'app/views'
end
```

`helpers_path` や `view_paths` で読み込み場所を調整できます。
これはうまくやれば省略できそうなのですがそこまでまだいけていません。

* [サンプルリポジトリはこちら。](https://github.com/eiel/ActienTemplate)

`ActionTemplate` を `app/models` に保存していますが、これは説明用です。
専用のディレクトリを作成して autoload_path へ追加したいですが、複雑になるので省略しました。

使い道があるかどうかわかりませんが、試したので整理してみました。

### 関連リンク

* [ActionView を単体で使ってみる](/blog/2014/07/18/action-view/)
