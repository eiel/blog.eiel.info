---

title: "ActiveModelを利用してフォームを作成した時の型変換"
date: 2013-03-20T15:00:00+09:00
comments: true
categories: ["ActiveRecord","ActiveModel","Ruby"]
---

対応するレコードがないフォームを使う場合、ActiveModelを使用することで、シンプルなビューを構築しつつ、処理はモデルにかけます。

しかし、ActiveModelのノウハウってあんまり落ちていません。
それなりに ActiveRecord に対する理解も必要で、難しいですね。
ハマったことなど共有していきたいと思います。


フォームからのデータは文字列ですが、ActiveRecord にはコラム自体には型があるため、型変換を自動的に行ってくれます。
これを無意識に使用していると ActiveModelではまります。

具体的には以下のテーブルがあったとします。

```ruby
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name
      t.integer :age
      t.boolean :is_person
      t.timestamps
    end
  end
end
```

利用例を見てみましょう。

```ruby
user = User.new(age: '20')
user.age                        # => 20
user.age.class                  # => Fixnum

user = User.new(is_person: "1")
user.is_person                  # => true
user.is_person.class            # => TrueClass
```

文字列から作成しているけども、自動的に数値や、真偽値へと変換されています。

ActiveModel を使用する場合は以下のように実装しておくとよさそうです。

```ruby
class User2
  attr_reader :age, :is_person
  include ActiveRecord::ConnectionAdapters

  def initialize(attributes = {})
    attributes.each do |key, value|
      send("#{key}=",value)
    end
  end

  def age=(age)
    @age = age.to_i
  end

  def is_person=(is_person)
    @is_person = Column.value_to_boolean(is_person)
  end
end
```

代入する時に値を修正するのが インスタンス変数に直接アクセスした場合にも型が保証できて良いです。

「別に文字列でもいいよ。」なんてこともあると思いますが、
数値だとおもってうっかり使うと `'20' * 3 -> '202020'` となって欲しい `40`とは大きく違います。
チェックボックスを利用すると `"1"` などなど、値として降ってきます。

特に 真偽値への変換ですが、とりあえずわからなかったので、自前でごまかしていたのですが、調べました。
`ActiveRecord::ConnectionAdapters::Column` にさままな型変換のメソッドが実装されています。


[https://github.com/rails/rails/blob/v3.2.13/activerecord/lib/active_record/connection_adapters/column.rb](https://github.com/rails/rails/blob/v3.2.13/activerecord/lib/active_record/connection_adapters/column.rb)

その中の `value_to_boolean` を使用しました。

```ruby
def value_to_boolean(value)
  if value.is_a?(String) && value.blank?
    nil
  else
    TRUE_VALUES.include?(value)
  end
end
```

`true` になる値は以下のように定義されてます。
```
[true, 1, '1', 't', 'T', 'true', 'TRUE', 'on', 'ON']
```

他にも

* value_to_integer
* value_to_decimal
* string_to_time
* string_to_date

などのメソッドがありました。


## まとめ

単体テストをかくときには文字列を渡すようにしたほうが良いかもしれません。
しかし、このような状況下になることはあまりないので、そんなに気にしなくても良いかなーという感じです。

テーブルつくれることならテーブルを作るほうが手軽そうです。

[サンプルコード](https://gist.github.com/eiel/5202625) はこちらに置いてます。

ActiveModel を使用するには 積極的にActiveRecord への理解を深める必要があります。
ソースコードもそこまで複雑ではないと思いますので、Rdocやソースコードも読んでいきたいですね。
