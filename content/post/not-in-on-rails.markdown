---

title: "Ruby on Rails で NOT IN な SQL をかく。"
date: 2013-07-20T15:53:00+09:00
comments: true
categories: ["rails4","rails3","rails", "arel"]
---

[Rails 4 で NOT な条件をもつ WHERE 句 が非常に書きやすくなりました。](http://guides.rubyonrails.org/active_record_querying.html#not-conditions)

Rails 4 なら NOT IN な SQL も簡単に書けます。

```ruby
User.where.not( name: ["hoge","goro"] )
```

条件にリストを渡せばよいです。SQLは以下のようになります。

```sql
SELECT "users".* FROM "users"
  WHERE ("users"."name" NOT IN ('hoge', 'goro'))
```

サブクエリも使えます。これが便利すぎて困る。

```ruby
query = User.select(:name)
User.where.not name: query
```

```sql
SELECT "users".* FROM "users"
  WHERE ("users"."name" NOT IN (SELECT name FROM "users"))
```

### Rails 3 の話

そうはいっても、Rails 4 にすぐには更新できないプロジェクトがあるので、これに慣れてしまうと非常につらい。折角なのでメモしておく。

#### where に文字列を渡す

格好悪いけど、とりあえずごまかす場合はこれを使う。

```ruby
User.where("name NOT IN ('hoge', 'goro')")
```

文字列で渡す。

`?` を使うとエスケープで泣きたくなるので、手軽には使えない。右辺も文字列で逃げます。

サブクエリを使うときは、こんな感じ。

```ruby
query = User.select(:name)
User.where("name NOT IN (#{query.to_sql})")
```

SQLを直接書くしかない。複雑な query だとつらいので `to_sql` メソッドで逃げます。

#### squeel を使う

Rails 3 だと NOT なSQL を書きづらいので、 [squeel](https://github.com/ernie/squeel) をよく使っています。

squeel を使っていればこんな風に書けます。

```ruby
User.where { name << ["hoge","mogu"] }
```

where に ブロックで引数が渡せるようになって `<<` という演算子を使うと NOT IN になります。`name` はシンボルではないのも ポイントです。

ブロックで渡すので、スコープ内で name に値が束縛されているとそちらを参照してしまうので注意。

サブクエリも使える。素晴らしい。

```ruby
query = User.select(:name)
User.where { name << query }
```

squeel を使うと LIKE やら OUTER JOIN やらもできます。便利。

#### arel の世界へ行く

arel で構築した sql を where に渡す方法がある。
squeel をインストールしたくない時に。

```ruby
users = User.arel_table
User.where( users[:name].not_in(['hoge','mogu']) )
```

arel 自体あまり解説してる人がいないのでなかなか勉強しにくいのが欠点。
わかってくるとなんとなくでも書けます。

サブクエリもいけます。

```ruby
query = User.select(:name)
users = User.arel_table
User.where( users[:name].not_in(query.arel) )
```

`ActiveRecord::Relation` はそのまま渡せませんが `arel` メソッドで変換可能です。

### まとめ

Rails 4 の `not` 便利です。

squeel も良いです.
私は最初は仕組みがわからなくて、気持ち悪かったです。
ところが、 `where` メソッドの実装を見たら察しがつきました。
利用する場合は、一度目に目にすることをおすすめします。

Rails3 からの `ActiveRecord::Relation` になれると元の世界に帰れる気がしません。

この記事のタイトルに悩んだけど、「書きはじめた理由でいいや」と、結局考えるのを放棄した。
