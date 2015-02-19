---

title: "最近やった Ruby でのミス - カンマで改行"
date: 2013-06-24T00:58:00+09:00
comments: true
tags: [ruby]
---

Ruby かいてて些細なミスでハマったことを書いておきます。

```ruby
goro = "gorogoro",
hoge = "hogehoge"
```

1行目の最後に カンマ が入っているのがポイントです。

期待した結果は

```ruby
goro # => "gorogoro"
hoge # => "hogehoge"
```

です。

実際には

```ruby
goro # => ["gorogoro", "hogehoge"]
hoge # => "hogehoge"
```

となりました。

1行目の最後にある カンマ を削除すれば期待した結果になります。

このミスは hash で値を渡していたところを 代入に書き換えたときに発生しました。異常はテストコードのおかげで直ちに検知できました。(ちらちら)


# 解説

カンマのあとの改行なので、 式が完結していないので以下と等価です。

```ruby
goro = "gorogoro", hoge = "hogehoge"
```

括弧をつけてわかりやすくします。

```ruby
goro = ("gorogoro", (hoge = "hogehoge"))
```

もうちょっとわかりやすくします。

```ruby
hoge = "hogehoge"
goro = "gorogoro", hoge
```

hoge が上にきているのに注意してください。

もう必要ないと思いますが、以下と等価です。

```ruby
hoge = "hogehoge"
goro = ["gorogoro", hoge]
```
