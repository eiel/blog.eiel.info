---

title: "ActiveRecordで関連レコードの自動保存"
date: 2012-12-30T13:50:00+09:00
comments: true
categories: ["rails","ruby"]
---

ActiveRecordで has_one なんかで関連づけしている場合、関連モデルを保存しわすれる。
そもそも、関連してることをドメインロジック上からは隠蔽したい。そんなときは `autosave` オプションが使えます。

関連するモデルが Information の場合、

```ruby
has_one :information, autosave: true
```

となります。
informationで親のモデル IDを validate presence かけてたらはまったことも一応メモしておきます。
