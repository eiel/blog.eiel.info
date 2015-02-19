---

title: "Rubyで次の水曜日の18時を取得する"
date: 2013-10-31T16:45:00+09:00
comments: true
categories: ['active_support']
---

日付処理って意外と面倒である。次の水曜日の18時を取りたい。


ActiveSupport を使っていいのであれば、このように書けた。

```ruby
require 'active_support/core_ext'
Date.today.beginning_of_week(:wednesday) + 1.week + 18.hours
```

3.0.0 だと `beginning_of_week` は引数が取れないので注意。3.0 と 4.0 でしか確認してない。
