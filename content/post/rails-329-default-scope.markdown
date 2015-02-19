---

title: "Rails 3.2.9 で default_scopeに設定してる条件が属性の初期値になるらしい"
date: 2012-11-16T15:05:00+09:00
comments: true
categories: ["rails","ruby","AcativeRecord"]
---

あるプロジェクトでrails 3.2.9 にアップデートしたら テストが失敗しまくる。そのひとつに ActiveRecordの default_scope を使ってる部分に問題があるとわかった。

どんなエラーかと言いますと。

```ruby
NoMethodError: undefined method `to_i' for [1, 2, 3]:Array
from activerecord-3.2.9/lib/active_record/connection_adapters/column.rb:178:in `value_to_integer'
```

`[1, 2, 3]` とか即値すぎて *わけがわからないよ* という感じだったんですが、いろいろ調べると

```ruby
class User < ActiveRecord::Base
   default_scope proc { where(state_id: [1, 2, 3]]) }
end
```

というコードがあったときに

```ruby
User.new
```

すると発生することがわかりました。

仕方ないので、
```ruby
class User < ActiveRecord::Base
   scope :valid, proc { where(state_id: [1, 2, 3]]) }
end
```
として、ひたすら置換しまくりでした。
僕はdefault_scope使わない派なのであまり気にしない方向で。


ちなみに
```ruby
class User < ActiveRecord::Base
   default_scope proc { where(state_id: 1,name: "hoge") }
end
```

としておくと

```ruby
User.new
=> #<User id: nil, name: "hoge", state_id: 1>
```
となるようです。
scopeから初期値を生成する機能がもともとあったみたいで(知らなかった)それが`default_scope`のものがデフォルトになったようです。
