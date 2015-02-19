---

title: "Rails で外部キー制約"
date: 2013-04-08T01:51:00+09:00
comments: true
categories: ["actirerecord","rails","database"]
---

外部キー制約は好きですか？

O/RマッパーしててもなるべくDBの力は借りたいです。
ということで、[Ruby Toolbox](https://www.ruby-toolbox.com/)で foreign を検索して、ちらちら見て一番人気の[foreigner](https://github.com/matthuhiggins/foreigner)を選びました。基本的にはデータベースに丸投げなので Rails4 でもいまのとこ問題なく使えております。


インストールは `Gemfile` に:

```ruby
gem 'foreigner'
```

と追加して `bundle install`

あとはマイグレーションで:

```ruby
create_table :comments do |t|
  t.references :post, null: false, index: true
  t.foreign_key :posts, dependent: :delete
end
```

としておきました。


ついでに確認。

存在しない 外部キーを指定すると保存を失敗する確認。postgresの例:
```ruby
Comment.create(post_id: 1)
# => PG::Error: ERROR:  insert or update on table "comments" violates foreign key constraint "comments_post_id_fk"
```

キーを指定して、親になるオブジェクトを削除すると消える確認:
```ruby
Comment.create(post_id: Post.create)
Comment.count
# => 1
Post.count
# => 1
Post.first.destroy
Post.count
# => 0
Comment.count
# => 0
```

ほむ。良い感じですね。

しかし、Rails4 からは scheme.rb の hash形式が変わってるけどそこには対応してませんでした。

```ruby
add_foreign_key "comments", "posts", :name => "comments_post_id_fk", :dependent => :delete
```

パッチがきてなかったら プルリクエストしてみようかな。
