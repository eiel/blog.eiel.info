---

title: "ActiveRecordで今のスコープをそのまま返したい"
date: 2012-08-01T11:37:00+09:00
comments: true
categories: [ruby,rails,ActiveRecord]
---

あるオプションパラメータがあるかどうかで、条件が変わるような処理を書いてると、オプションがない場合、ActiveRecord::Relationが欲しくなるような場面があります。

例えば

```ruby
@articles = Article
@articles = @articles.where(valid: true) if params[:valid]

```
みたいな感じになっちゃって`@articles = Article`って何?な状態になります。

メソッド化しようとするとさらに困るのですが、scopedを使うと以下のように書けるようです。

```ruby
@articels = Article.scoped
@articles = @articles.where(valid: true) if params[:valid]
```

なんか異臭がしなくなりましたね。

だから、どうした？って思う方もいるかもしれませんがメソッド化すると、

```ruby
def self.valid(is_valid = nil)
  scoped.where(valid: true) if is_valid
  scoped
end
```
となります。`scoped`なしで書かこうとするとちょっと困ります。

そんだけ。
