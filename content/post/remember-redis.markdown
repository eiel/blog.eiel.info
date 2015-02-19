---

title: "Redis に保存されてる値を見ようと思った時に覚えておきたい redis コマンド"
date: 2014-08-26T17:54:00+09:00
comments: true
categories: [redis]
---

[redis](http://redis.io/) なんか使ったことないけど redis を使うアプリケーションを使う場合に、デバッグや動作確認をするのに、データベースの中身が知りたい場合がある。

そんな時に覚えておきたいことを整理してみた。

自分が hubot や sensu の動きを確認するのに必要だったことを書いてるだけで、 redis のことはよく知らない。

コマンドラインで redis にアクセスするには redis-cli コマンドを使用する。

redis-cli を実行して、対話環境を利用してもいいし、引数を追加して実行することもできる。

最初に紹介する keys コマンドを例にすると

```
$ redis-cli
> keys *
```

```
$ redis-cli keys "*"
```

といったアクセス方法がある。

ここで紹介するコマンドの一覧

コマンド | 説明
-- | ---
keys * | redisに登録されているキーの一覧を取得する key のパターンを指定する
type [key] | value の種類を返す。
get [key] | type が string だった場合の値をみる方法
lrange [key] 0 -1 | type が list だった場合の値をみる方法
smembers [key] | type が set だった場合の値をみる方法
zrange [key] 0 -1 | type が zsetだった場合の値をみる方法
hgetall [key] | type が hash だった場合の値をみる方法
hkeys [key] | type が hash だった場合に field の一覧をみる方法
hvals [key] | type が hash だった場合に value の一覧をみる方法
monitor | redisサーバが受けとったコマンドを表示する


設定したりもっと細かい作業をしたい場合は help コマンドを使う。
種類ごとのヘルプをみたい場合は `@` をつけるとよい

例えばリスト関連のコマンドを知りたいなら

```
> help @list
```

といった感じ。

以下は解説

### keys

登録されている key がわからないと何もできないので、keyの一覧をみる方法

```
> keys *
```

引数にはパターンを入力する hogeではじまるものに絞りこみしたい場合は

```
> key hoge*
```

とかする。shell の場合はアスタリスクはエスケープする必要があるのに注意

```
$ redis-cli keys \*
```

### type

redis は key に格納された値の種類によって取得コマンドが違うらしい。
値をみるために種類の確認が必要。

hoge というキーがあった場合は

```
> type hoge
```

とする。

返す値としては

* string
* list
* set
* zset
* hash

があるっぱい。

### string だった場合

キーとキーの種類が別れば値を見ることができる。

キーhoge の値の種類が string だったら get を使う

```
> get hoge
```

ちなみに設定する場合は set。hubot は json が string で保存してあった。

### list だった場合

キーhoge の値の種類が list だった lrange を使う。

```
> lrange hoge 0 -1
```

先頭 から 最後から1番目までの値を返せという感じになる。
必要な範囲を指定したい場合は微調整するとよい。

### set だった場合

キーhoge の値の種類が set だった場合は smembers を使う。

```
> smembers hoge
```

### zset だった場合

キーhoge の値の種類が zset だった場合は zrange を使う。
zset 順序のある集合

```
> zrange hoge 0 -1
```

list の時と同じ。

### hash だった場合

キーhoge の値の種類が hash だった場合は hgetall を使う。
ただし、field と value が交互に表示されてよみやすくはない。

```
> hgetall hoge
```

field だけや value だけの一覧をみたい場合は

```
> hkeys hoge
> hvals hoge
```

がある。

なぜ hfields ではないのか。

### アクセスされてるのか確認したい時

ログをみてもいいのかしれないけど `monitor` コマンドが便利。
データベース内で何が起きているかを理解するのに良いらしい。

tail -f みたいな感じ。

### まとめ

これだけ知っていれば十分に redis の中身を見ることができる気がする。

詳しいことは help コマンドとか redis のサイトを見よう。

[日本語訳もある](http://redis.shibu.jp/)
