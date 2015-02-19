---

title: "ActionView を単体で使ってみる"
date: 2014-07-18T18:35:00+09:00
comments: true
tags: ["rails","actionview","ruby"]
---

誰が興味があるのか謎ですが、[ActionView](https://github.com/rails/rails/tree/v4.1.4/actionview) を単体で使ってみようと思います。
意外にも Rails の仕組みとか見えてくるかもしれません。

Rails 4.1 ぐらいから ActionPack から独立した記憶があります。どうでしたっけ。

テンプレートを使いたい時には erb, haml, slim などを単体で利用すればいいのであまり使う機会はないかもしれません。

雑感では、

* layout 機能を使いたい
* インスタンス変数で値にアクセスしたい
* Rails が提供するビューヘルパーを使いたい

あたりがメリットかと思います。

この記事のために[作成したコードはこちら](https://github.com/eiel/use-actionview)においておきます。

補足の部分は読み飛ばせるように書いているつもりです。

利用したRailsのバージョンは 4.1.4 です。

### 1 Hello, world

まずは使ってみます。

```ruby
ActionView::Base.new.render(inline: 'Hello, World!') # => Hello, world
```

[ActionView::Base](https://github.com/rails/rails/blob/v4.1.4/actionview/lib/action_view/base.rb) のインスタンスを作成し、renderメソッドを呼びだします。
コントローラでの render メソッドはどうやらこの render メソッドのようです。

(viewで使う render もこの render ですが…)

### 1の補足 ActionView::Base

Rails を使ってる際に erb ファイルの中で `self.class` を確認したことはあるでしょうか？

ちょっと確認してみましょう。

```
<%= self.class %>
<%= self.class.superclass %>
```

```
#<Class:0x007f82891092e0>
ActionView::Base
```

self は無名のクラスになっていますが、そのスーパークラスは ActiovView::Base です。
ビューは ActionView::Base のインスタンスのコンテキストで実行されるわけです。ビューコンテキストと呼んでいるようです。

また、このクラスにヘルパーをミックスインすることでヘルパーとして利用できるようになります。

デフォルトのHelperはすでに include されています。

```
> ActionView::Base.ancestors.map(&:to_s).grep(/Helper/)
=> ["ActionView::Helpers", "ActionView::Helpers::TranslationHelper", "ActionView::Helpers::RenderingHelper", "ActionView::Helpers::RecordTagHelper", "ActionView::Helpers::OutputSafetyHelper", "ActionView::Helpers::NumberHelper", "ActionView::Helpers::JavaScriptHelper", "ActionView::Helpers::FormOptionsHelper", "ActionView::Helpers::FormHelper", "ActionView::Helpers::FormTagHelper", "ActionView::Helpers::TextHelper", "ActionView::Helpers::DebugHelper", "ActionView::Helpers::DateHelper", "ActionView::Helpers::CsrfHelper", "ActionView::Helpers::ControllerHelper", "ActionView::Helpers::CacheHelper", "ActionView::Helpers::AtomFeedHelper", "ActionView::Helpers::AssetTagHelper", "ActionView::Helpers::AssetTagHelper::StylesheetTagHelpers", "ActionView::Helpers::AssetTagHelper::JavascriptTagHelpers", "ActionView::Helpers::SanitizeHelper", "ActionView::Helpers::ActiveModelHelper", "ActionView::Helpers::UrlHelper", "ActionView::Helpers::TagHelper", "ActionView::Helpers::CaptureHelper"]
```

### 2 インスタンス変数を使う

Rails ではコントローラのインスタンス変数がビューの中で使えます。
普段はRailsが自動でやってくれていますが、自分でインスタンス変数を設定するには assign メソッドを使います。

```ruby
view_context = ActionView::Base.new
view_context.assign(name: 'eiel')
view_context.render(inline: 'Hello, <%= @name %>') # => Hello, eiel
```
`@name` が eiel に展開されています。

ActionView::Base のコンストラクタの第2引数に渡しても設定できます。

### 2の補足 ActionView::Rendering

コントローラがビューコンテキストに対して assign メソッドを利用して、設定します。
これは [ActionView::Rendering](https://github.com/rails/rails/blob/v4.1.4/actionview/lib/action_view/rendering.rb) で行われます。

この ActionView::Rendering には ActionController::Base にミックスインされていて、コントローラがビューを設定する処理などが記述されているようです。

```
> ActionController::Base.ancestors.map(&:to_s).grep(/ActionView/)
=> ["ActionView::Layouts", "ActionView::Rendering", "ActionView::ViewPaths"]
```

ActionController::Base には ActionView::Rendering がミックスインされています。

ちなみに assign するのに使う Hash は AbstractController::Rendering#view_assign で作成されています。

```ruby
def view_assigns
  protected_vars = _protected_ivars
  variables      = instance_variables

  variables.reject! { |s| protected_vars.include? s }
  variables.each_with_object({}) { |name, hash|
    hash[name.slice(1, name.length)] = instance_variable_get(name)
  }
end
```

* [ソースコード](https://github.com/rails/rails/blob/53d7b2ffe9ccdf2ded9898e20a947ea7da63566e/actionpack/lib/abstract_controller/rendering.rb#L66-L74)

インスタンス変数の一覧を取り出し、先頭の `@` を取り除いてハッシュにしています。_protected_ivars に登録されているものは除外されます。

### 3 テンプレートファイルの利用

別のファイルに保存したテンプレートを利用してみます。
ActionView::LookupContext というものがテンプレートファイルを探します。

`views/prefix/hoge.html.erb` を用意して中身は

```
Hello, <%= @name %>
```

として用意しているとします。

使ってみます。

```ruby
require 'action_dispatch/http/mime_type'
view_context = ActionView::Base.new('./views')
view_context.assign(name: 'eiel')
view_context.render(template: 'hoge', prefixes: 'prefix') # => Hello, eiel
```

ActionView::Base の第一引数から自動的に ActionView::LookupContext が生成されます。

バグなのかどうか判断が付いていないですが action_dispatch/http/mime_type を読まなりと動いてくれません。

どうしても読みたくない場合は以下のようにします。

```ruby
lookup_context = ActionView::LookupContext.new('./views')
lookup_context.cache = false   # ActionPack を読まなくて済む魔法

view_context = ActionView::Base.new(lookup_context)
view_context.assign(name: 'eiel')
view_context.render(template: 'hoge', prefixes: 'prefix') # => Hello, eiel
```

ルックアップコンテキストを自分で作り、cache を切ると ActionDispatch を利用せずに動かすことができます。

Rails が prefixes と template を自動で設定してくれていることが想像できます。普段はコントローラ名やアクション名から判断できるからですね。

prefixes は指定しないとテンプレートをみつけることができないようです。

また、文字列を指定することもできます。

```
view_context.render('prefix/hoge')
```

この場合は `prefix/_hoge.html.erb` のようなファイルを探しにいきます。

### 3の補足

render に自動設定されるオプションは _normalize_options メソッドで設定されるようです。

例えば  [ActionVIew::Rendreing#_normalive_options](https://github.com/rails/rails/blob/v4.1.4/actionview/lib/action_view/rendering.rb) では

```ruby
def _normalize_options(options)
  options = super(options)
  if options[:partial] == true
    options[:partial] = action_name
  end

  if (options.keys & [:partial, :file, :template]).empty?
    options[:prefixes] ||= _prefixes
  end

  options[:template] ||= (options[:action] || action_name).to_s
  options
end
```

となっていて prefixes や template が設定されている様子があります。

特に `options[:template] ||= (options[:action] || action_name).to_s` なんかは予想通りな感じですね。
options に :action を利用して、なければ action_name を利用しています。

prefixes は [ActionView::ViewPaths](https://github.com/rails/rails/blob/7b50d7f2496a84bec5aceb9e0fd1f1f9dcbdab88/actionview/lib/action_view/view_paths.rb#L34-L36) で

```ruby
def local_prefixes
  [controller_path]
end
```

となっており、

```ruby
def _prefixes # :nodoc:
  @_prefixes ||= begin
    deprecated_prefixes = handle_deprecated_parent_prefixes
    if deprecated_prefixes
      deprecated_prefixes
    else
      return local_prefixes if superclass.abstract?

      local_prefixes + superclass._prefixes
    end
  end
end
```

最終的に _prefixes として利用できることがわかります。

そういえば ActionView::ViewPaths も ActionController::Base にミックスインされていましたね。

```
> ActionController::Base.ancestors.map(&:to_s).grep(/ActionView/)
=> ["ActionView::Layouts", "ActionView::Rendering", "ActionView::ViewPaths"]
```

### 4 レイアウトの利用

レイアウトを利用するには `layout` オプションを使います。

```
$ tree views/
views/
├── layouts
│   └── application.html.erb
└── prefix
    └── hoge.html.erb
$ cat view/layouts/application.html.erb
--
<%= yield %>
--
```

としておいて、

```ruby
lookup_context = ActionView::LookupContext.new('./views')
lookup_context.cache = false   # ActionPachk を読まなくて済む魔法

view_context = ActionView::Base.new(lookup_context)
view_context.assign(name: 'eiel')
view_context.render(template: 'hoge',
                          prefixes: 'prefix',
                          
```

とすると、

```
--
Hello, eiel

--
```

のような文字列がかえってきます。

### 4の補足

layout に関するコントローラの処理は [ActionView::Layouts](https://github.com/rails/rails/blob/v4.1.4/actionview/lib/action_view/layouts.rb) にあります。

もう一度確認してみましょう。ActionController::Base にミックスインされているモジュールを確認してみましょう。

```
> ActionController::Base.ancestors.map(&:to_s).grep(/ActionView/)
=> ["ActionView::Layouts", "ActionView::Rendering", "ActionView::ViewPaths"]
```

ちなみに ActionView::Rendering は ActionView::Layouts で `include` されています。

render へのオプション設定はやっぱり `_normalaize_options` にあります。

```
def _normalize_options(options) # :nodoc:
  super

  if _include_layout?(options)
    layout = options.delete(:layout) { :default }
    options[:layout] = _layout_for_option(layout)
  end
end
```

`options[:layout]` を設定しています。

### まとめ

ActionView を単体で使いたい場面を考えると ERB を単体で利用していたけど layout を使いたくなったときぐらいしか浮かびません。
Rails が提供する MVC に乗かりたいなら AbstroctController を使うほうが楽そうです。

補足としたほうを読んでみると Rails の仕組みも見えてくるような気がしますね。(#知らんけど)

### 補足のまとめ

登場人物を整理しておきます。

* view_context - ActionView::Base のサブクラスのインスタンス。ビューの中のself
* lookup_context - ActionView::LookupContext のインスタンス。テンプレートを探してくれる。テンプレートを探すための情報ももってる。
* renderer - render を実際に行うところ。今回は登場してない。render の引数によってどのクラスを使うか選択される。

コントローラーと連携するためにコントローラに機能を追加する人たちとして、

* ActionView::Layouts
* ActionView::Rendering
* ActionView::ViewPaths

が登場しました。

### 関連

* [AbstractController を継承して遊ぶ](/blog/2013/09/04/extend-abstract-controller/)
* [ActionDispatch ってなんだろう？ - 広島・岡山Ruby交流会01](blog/2014/03/30/action-dispatch/)
* [Rails の自動読み込みの話](/blog/2013/09/07/autoload-rails/)
