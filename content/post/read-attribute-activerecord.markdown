---

title: "read_attributeの存在を知らなかった、死にたい - rails"
date: 2012-12-17T16:57:00+09:00
comments: true
tags: ["rails","ruby","activerecord"]
---

Railsの ActiveRecordで レコードの属性にアクセスする際は動的に生成されたメソッドを使いますが、そのようなメソッドを上書きしている場合、値に直接アクセスする必要があります。このような属性情報は `@attributes` に保存されています。

`/lib/active_record/attribute_methods.rb`に定義されてる attributes メソッドを経由してアクセスしていましたが、なんとなく `@attributes` へ直接アクセスするだけかとおもってたのですが、違ったようです。

```ruby
def attributes
  attrs = {}
  attribute_names.each { |name| attrs[name] = read_attribute(name) }
  attrs
end
```

という定義になってました。

`attribute_names` は文字列で属性の一覧を返すので `@attributes`は 普通のHashでキーが文字列です。
もし `email` というの属性にアクセスしたい場合は `attributes["email"]` になります。 `attributes[:email]` ではアクセスすることができません。

しかし、 read_attributeは シンボルでも文字列でも使用することができて、
`read_attribute :email` でも `read_attribute "email"` のどちらでも良いみたいです。

ちなみにエイリアスがあって `[]` メソッドになります。なので `self[:email]` などでアクセスできます。pubilcメソッドです。

`read_attribute` があるということはも `write_attribute` もあります。

### ついでにもう少し深追い 

`read_attribute`の実装もついでにおってみると

```ruby
def read_attribute(attr_name)
  self.class.type_cast_attribute(attr_name, @attributes, @attributes_cache)
end
```

となってました。クラスメソッドを経由するようです
こいつも中身を追うと


```ruby
def type_cast_attribute(attr_name, attributes, cache = {}) #:nodoc:
  return unless attr_name
  attr_name = attr_name.to_s

  if generated_external_attribute_methods.method_defined?(attr_name)
    if attributes.has_key?(attr_name) || attr_name == 'id'
      generated_external_attribute_methods.send(attr_name, attributes[attr_name], attributes, cache, attr_name)
    end
  elsif !attribute_methods_generated?
    # If we haven't generated the caster methods yet, do that and
    # then try again
    define_attribute_methods
    type_cast_attribute(attr_name, attributes, cache)
  else
    # If we get here, the attribute has no associated DB column, so
    # just return it verbatim.
    attributes[attr_name]
  end
end
```

渡された名前がすぐに文字列に変換されてます。

そしてまず 属性にアクセスするためのメソッドがあるかどうか確認するようです。ある場合はそちらに処理を渡すようです。attributesにまわってくるオブジェクトがHashじゃないモデルオブジェクトの場合の処理っぽいです。
また、まだ未定義なだけな場合は定義してからアクセスするようです。
それ以外のただのhashの場合は直接アクセスしにいくようです。

ここでの attributes はActireRecordの attributes メソッドではなくただのHashです。一瞬、無限ループしてるように思えたので一応書いておきます。
