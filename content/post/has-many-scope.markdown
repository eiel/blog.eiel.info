---

title: "ActiveRecord の has_many で生成されるメソッドってActiveRecord::Relationに変換できる配列なんですね。"
date: 2013-01-25T00:34:00+09:00
comments: true
tags: ["rails","ActiveRecord","ruby"]
---

タイトルのとおりなんですが、`Article`と`Comment` とかあったりして、ちゃんと設定をしておくと `article.comments` とやると `あるArticle`に紐づいている`Comment`がとってこれる機能です。

まず、結論からいうと `article.comments.to_sql` とか `article.comments.scoped` とか `article.comments.joins`とかできる!! ということです。

`article.comments.create` ってかける時点でうすうす思ってたんですが、これがわかっていると小回りがききます。返しているものが `ActiveRecord::Relation`のようなものです。`class`を確認すると `Array`って言われちゃいますが。

## もうちょい深く

以下のクラスがあることを想定してみます。

```
class Article < ActiveRecord::Base
  has_many :comments
end
```

```
class Comment < ActiveRecord::Base
  belongs_to :artcile
  belongs_to :user
end
```

```
class User
  has_many :comment
end
```

さきほど紹介した技を紹介すると User.first かつ Article.first な Commentを探す場合、以下のように書けます

```ruby
a = Article.first
u = User.first
a.comments.merge(u.comments.scoped)
```

すると、こんな SQLができます。
```sql
SELECT "comments".* FROM "comments"  WHERE "comments"."article_id" = 1 AND "comments"."user_id" = 1
```
aとかuとかを引数な関数を用意するとウハウハな気がしてこないでしょうか。

joinだってできます。
```ruby
a = Article.first
a.comments.merge(Comment.joins(:user))
```

```sql
SELECT "comments".* FROM "comments" INNER JOIN "users" ON "users"."id" = "comments"."user_id" WHERE "comments"."article_id" = 1
```
これは has_many through でもできますね。

このあたりを上手くつかっていけば ActiveRecordでも作りたいSQLがある程度つくれるんじゃないでしょうか。

Rails4がくると scoped をかかなくてもよくなるような気がしますが、試していません。

しかし、はじめの例ですが、
```ruby
a.comments.where(user_id: u)
```
「ってかいても同じじゃね？」とか、言わないでください。なんとなく `user_id` ってかきたくなくないですか？

## なんとなくおまけ

a.comments とかいた場合は Commentの Relationをつくっている。

u.comments とかいても Commentの Relationをつくっている。

と、メソッド名のほうのテーブルを意識してやると理解しやすいと思います。

レシーバほうに対してのテーブルを意識すると息苦しくなります。`has_many`をかく場合はそういう意識になるのでちょっと注意が必要です。

ちょんと感覚的な話でした。

## サンプル用コード

動作確認のためのコードを用意しておきました。おすきにお使いください。

[https://github.com/eiel/has_many-relation](https://github.com/eiel/has_many-relation)
