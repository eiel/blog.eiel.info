---

title: "TagHelperっていうのがあるんだけど、周りの人が使ってない - Rails"
date: 2012-11-22T10:35:00+09:00
comments: true
categories: ["rails","ActionPack"]
---

Railsのhelperに [TagHelper](http://rubydoc.info/gems/actionpack/3.2.8/ActionView/Helpers/TagHelper) っていうのが `text_area_tag` のようなフォームを作成するような Helper がありますが、その内部で使用するようなメソッドが定義されています。

主に `tag`, `content_tag` になるのですが、自分でHTMLのタグを生成するようなヘルパーを生成したときこれを使うと便利です。

例えば `hoge`的なものを表現するタグを生成するhoge_tagという抽象化をしたいときにざっくりな実装をすると以下のようになります。
```ruby
def hoge_tag(content)
  %Q{<div class="hoge">#{content}</div>}.html_safe
end
```

```ruby
hoge_tag "hogehoge" # => "<div class=\"hoge goro\">hogehoge</div>"
```

しかし、つかっていくと個別に class属性を追加したくなる場合が多々ありますし、content の escape などもしないといけないです。

```ruby
def hoge_tag(content, *classes)
  classes = ["hoge"] + classes
  class_string = classes.join(" ")
  %Q{<div class="#{class_string}">#{content}</div>}.html_safe
end
```

```ruby
hoge_tag "hogehoge", "goro" # => "<div class=\"hoge goro\">hogehoge</div>"
```

もちろん、class属性だけじゃなくていろいろ指定したくなります。
`text_area_tag` なんかと同じようにoptionsで受けるようにしたい。こうなっとときに 「**ソースを読もう!!**」 という発想が出るようになると良いと思います。

```ruby
def hoge_tag(content, options = nil)
  classes = options[:class]
  classes = [classes] unless Array === classes
  options[:class] = ["hoge"] + classes
  content_tag :hoge, content, options
end
```

```ruby
hoge_tag "hogehoge", class: "goro"
```

content_tag はブロックを受けとったり, escapeの可否などの指定もできます。
ブロックをうけとるようなものはテンプレートエンジン内ではなかなかあばれてくれます。


```
<%= content_tag :hoge do %>
  "hogehoge"
<% end %>
```
といった書き方ができます。

`<br />`,`<img />`のような内部にコンテントを持たないタグを生成する場合は `tag`というメソッドを使うと良いです。

Viewを書きはじめる前に [Helperの一覧](http://rubydoc.info/gems/actionpack/3.2.8/ActionView/Helpers)をみておくのも良いと思います。
