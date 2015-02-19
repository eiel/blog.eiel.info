---

title: "ActionView 単体で slim を使ってみる"
date: 2014-07-28T15:52:00+09:00
comments: true
tags: ["actionview","rails","slim"]
---

「[ActionView を単体で使ってみる](/blog/2014/07/18/action-view/)」というのを書いたので、ついでにいろいろ試してみる。その1。

誰が得するのか謎だけど ActionView だけで slim を使うことを試みてみました。
`action_view`を require して、 `action_pack` を require して、 `slim` を require すれば使えます。

用意したファイル

* `views/prefix/slim.html.slim`

```
p
  | Hello, #{@name}
```

* `views/layout/appliacation.html.erb`

```
--
<%= yield %>
--
```


```ruby
require 'action_view'
require 'action_pack'
require 'slim'

lookup_context = ActionView::LookupContext.new('./views')
lookup_context.cache = false   # ActionPachk を読まなくて済む魔法

view_context = ActionView::Base.new(lookup_context)
view_context.assign(name: 'eiel')
ret = view_context.render(template: 'slim',
                          prefixes: 'prefix',
                          
puts ret

=begin
--
<p>Hello, eiel</p>
--
=end
```

slim は [temple](https://github.com/judofyr/temple) という gem を使ってRailsに対応してました。
ActionPack はバージョン確認に利用しているだけなので、ちょっといじればなんとかなりそうですけど。

* [Temple::Templates::Rails](https://github.com/judofyr/temple/blob/v0.6.8/lib/temple/templates/rails.rb)

### もう少し詳しく

誰得感がひどいのでもうちょっと書いてみる。

Railsとの連携の処理の部分は

```ruby
Temple::Templates::Rails(Slim::Engine,
  :register_as => :slim,
  # Use rails-specific generator. This is necessary
  # to support block capturing and streaming.
  :generator => Temple::Generators::RailsOutputBuffer,
  # Disable the internal slim capturing.
  # Rails takes care of the capturing by itself.
  :disable_capture => true,
  :streaming => defined?(::Fiber))
```

* [Slim::Template](https://github.com/slim-template/slim/blob/master/lib/slim/template.rb#L9-L17)

そうすると `Temple::Template#method_missing` が呼ばれてます。

```ruby
def self.method_missing(name, engine, options = {})
  const_get(name).create(engine, options)
end
```

* [Temple::Temlate#method_missing](https://github.com/judofyr/temple/blob/v0.6.8/lib/temple/templates.rb#L7-L9)

変数を置き換えてみると

```ruby
Temple::Templates::Rails.create(Slim::Engin,
  register_as: :slim,
  generator: Temple::Generators::RailsOutputBuffers,
  disable_caputre: true,
  streaming: defined?(::Fiber))
```

と [Temple::Templates::Rails.create](https://github.com/judofyr/temple/blob/v0.6.8/lib/temple/mixins/template.rb#L17-L25) が  `register_as: :slim` 呼ばれることがわかります。

```ruby
def create(engine, options)
  register_as = options.delete(:register_as)
  template = Class.new(self)
  template.disable_option_validator!
  template.default_options[:engine] = engine
  template.default_options.update(options)
  template.register_as(*register_as) if register_as
  template
end
```

そうすると [Temple::Templates::Rails.register_as](https://github.com/judofyr/temple/blob/v0.6.8/lib/temple/templates/rails.rb#L41-L45) が `ActionViewActionView::Template.register_template_handler` を呼びだされて、ActionView で利用できるようになります。

names には `[:slim]` が束縛されることになります。

```ruby
def self.register_as(*names)
  names.each do |name|
    ActionView::Template.register_template_handler name.to_sym, new
  end
end
```

### まとめ

ActionViewにテンプレートエンジンを追加するには `ActionView::Template.register_template_handler` 使うことがわかりました。

ちなみに Rails のリポジトリを検索するとこんな感じでした。

```ruby
base.register_default_template_handler :erb, ERB.new
base.register_template_handler :builder, Builder.new
base.register_template_handler :raw, Raw.new
base.register_template_handler :ruby, :source.to_proc
```

* [ソースコード](https://github.com/rails/rails/blob/v4.1.4/actionview/lib/action_view/template/handlers.rb#L10-L13)


### 関連

* [ActionView を単体で使ってみる](/blog/2014/07/18/action-view/)
