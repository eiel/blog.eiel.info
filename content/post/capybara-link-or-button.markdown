---

title: "Cucumber の Capybara で 複数の同じ名前のリンクに対応するステップ"
date: 2013-01-27T00:12:00+09:00
comments: true
categories: ["rails","cucumber","capybara"]
---

Cucumber のステップで
```ruby
もし /^"(.+)"をクリック$/ do |name|
  click_on name
end
```

というステップを書いていますが、`name` に複数マッチしてしまうとエラーが発生しています。同じ名前にならないようにすればいいのですが、そうもいかない場合もあります。結局、以下の方法を用意しました。

```ruby
もし /^(\d+)番目の"(.*?)"をクリック_$/ do |n, name|
  n = n.to_i - 1
  all(:link_or_button, name)[n].click
end
```

何番目のリンクか指定することで回避しました。

## もうちょっと詳しく

`click_button`と同じことをやろうとすると `find(name)` や `all(name)` ではうまくいきません。調べてみると `XPath::HTML.link_or_button` というメソッドを使用して、[findに渡すXPathを生成してることがわかりました。](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/selector.rb#L90-L93)。

これをどうやって使うというと all の第一引数に使えばいいことがわかりました。

## さらに詳しく


```ruby
page.class # => Capybara::Session
```
click_onメソッドやallメソッドのレシーバである page オブジェクトは Capybara::Session でした。pry で調べました。

* [Capybara::Session](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/session.rb)

```ruby
    NODE_METHODS = [
      :all, :first, :attach_file, :text, :check, :choose,
      :click_link_or_button, :click_button, :click_link, :field_labeled,
      :fill_in, :find, :find_button, :find_by_id, :find_field, :find_link,
      :has_content?, :has_text?, :has_css?, :has_no_content?, :has_no_text?,
      :has_no_css?, :has_no_xpath?, :resolve, :has_xpath?, :select, :uncheck,
      :has_link?, :has_no_link?, :has_button?, :has_no_button?, :has_field?,
      :has_no_field?, :has_checked_field?, :has_unchecked_field?,
      :has_no_table?, :has_table?, :unselect, :has_select?, :has_no_select?,
      :has_selector?, :has_no_selector?, :click_on, :has_no_checked_field?,
      :has_no_unchecked_field?, :query, :assert_selector, :assert_no_selector
```

```ruby
NODE_METHODS.each do |method|
  define_method method do |*args, &block|
    @touched = true
    current_node.send(method, *args, &block)
  end
end
```

[https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/session.rb#L338-L343](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/session.rb#L338-L343)

これらの メソッドは動的に生成されるようです。

```ruby
def current_node
  scopes.last
end

def scopes
  @scopes ||= [document]
end
```
[https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/session.rb#L351-L358](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/session.rb#L351-L358)

current_node は document という変数に格納されたオブジェクトのようです。

```ruby
def document
  @document ||= Capybara::Node::Document.new(self, driver)
end
```

[https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/session.rb#L334-L336](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/session.rb#L334-L336)

documentは Capybara::Node::Document クラスのインスタンスだとわかりました。

```ruby
class Document < Base
```

[https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/node/document.rb#L11](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/node/document.rb#L11)

ここには all メソッドがなく Capbyra::Node::Base を継承しているようです。


```ruby
include Capybara::Node::Finders
```

[https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/node/base.rb#L27](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/node/base.rb#L27)

この辺にありそうですね。

```
      def click_link_or_button(locator)
        find(:link_or_button, locator).click
      end
      alias_method :click_on, :click_link_or_button
```
[https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/node/actions.rb#L12-L15](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/node/actions.rb#L12-L15)

cloick_on みつけました。このあたりを grep でみつけた時点で `all :link_or_button` でいけそうなのはわかります。

```ruby
def all(*args)
  query = Capybara::Query.new(*args)
  elements = synchronize do
    base.find(query.xpath).map do |node|
      Capybara::Node::Element.new(session, node, self, query)
    end
  end
  Capybara::Result.new(elements, query)
end
```

[https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/node/finders.rb#L110-L118](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/node/finders.rb#L110-L118)

all みつけたー!


* [Capybara::Query.initialize](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/query.rb#L14-L16)

第一引数がシンボルの時の処理がありました。

## ごめん力尽きた。

* [Capybara::Selector.add](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/selector.rb#L11-L13)

生成したXpathをハッシュに保存しておく

* [Capybara::Selector.all](https://github.com/jnicklas/capybara/blob/2.0.2/lib/capybara/selector.rb#L7-L9)

登録しておいたxpathを撮りだす。


という感じです。実際にはpryの show-method つかって探してるのでこんなに大変じゃないんだからね!
