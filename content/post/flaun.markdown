---

title: "ssh でポートフォワーディングしてブラウザで開くだけの gemを作った"
date: 2013-08-10T01:57:00+09:00
comments: true
tags: ["ruby"]
---


[flaun](https://github.com/eiel/flaun) という gem つくって公開してみた。
インストールするには `$ gem install flaun` でできます。

何をするgemかというと

```
ssh example.jp -L 8000:localhost:80
open http://localhost:8000/
```

のようなことをするだけの gem です。

`~/.flaun` ファイルに設定を書いておくと `flaun [target名]` で該当サイトが開けます。
[target名] は ID みたいなものでどれを開くか指定するためのものです。
設定はRubyで作った DSL でかけます。

sample という target名の設定は以下のようになります。

```ruby
port 8000

target :target do
  host 'example.jp'
end
```

はじめから `http://localhost:8000/foobarr` のようにディレクトリなどpathを指定したい場合は

```ruby
port 8000

target :target do
  host 'example.jp'
  path 'foobar'
end
```

のように書けます。

### どんな時に使うの

アクセス制限かけたいけど固定IPがない。BASIC認証はちょっと…。
そんな時は `localhost` からのみアクセスしたいサーバがあることがあります。

Apache だと以下のように書いてるページですね。

```
Order allow,deny
Allow from localhost
```

こうなるとサーバからアクセスしないと表示することができません。
こんな時にポートフォワーディングを使います。
これを簡単に開きたい。そういう時に普段からコマンドライン操作をしていれば便利です。

### 中身の話

ホスト名などの設定値を固定値でやるのは簡単だったけど、設定を DSL でかけるようにしました。
DSLは BasicObject クラスを継承してそこにメソッドを定義しておき ファイルをread した文字列を クラスのインスタンスで instance_eval することで実現しています。

* [ソースコード](https://github.com/eiel/flaun/blob/v0.0.2/lib/flaun/dsl.rb)

DSLクラスがトップレベル用の DSL を定義していて DSLTarget が target ブロック内でのDSL を定義しています。
このメソッドが呼ばれた際にconfigオブジェクトを構築していき、実行終了に config オブジェクトを取り出して使うようになっています。

DSLクラスは

```ruby
def port(port)
  @config.port = port
end

def target(name, &block)
  target = DSLTarget.new(@config)
  target.instance_exec &block
  @config.targets[name] = target.config
end
```

となっているので `port` と `target` という命令があります。
target ブロックの中で使えるものは DSLTarget のメソッドを見ればわかります。そうして最終的には

```
config
  port
  targets:
    [target名1]:
	  port:
	  host:
	  path:
	[target名2]:
	  port:
	  host:
	  path:
```

という感じのデータ構造のオブジェクトが生成されます。

コマンドの第1引数に指定された target名を取得して命令を作成者し、実行するようになっています。
オプションを追加したいのであればこの辺をいじれば可能です。

他には ssh のセッションは別のスレッドで実行し、実行しつつ入力を受けつけるようにしてみました。
スレッドを使うとデバッグがしづらいですね。
pryで止めると入力がプログラムに食われたりしてめんどくさかったです。

cucumber でシナリオをひとつだけ書いて、作成してみましたが、なかなかかゆいところに届きませんね。
最初は [aruba](https://github.com/cucumber/aruba) という コマンドラインツールのためのcucumber拡張を試していましたが、スタブがかけづらくてつらいのではずしました。

### まとめ

「具体的なパラメータが入っていて公開できないけど、ちょっとしたスクリプトかく」ということは、よくありますがうまく分離できなくてなかなか公開できないです。
積極的に公開して解説を書いておくことで、忘れたことに治したい場合、とても便利です。

自分のためにつくってるので予想してない使い方などするとエラーが出ると思います。
[Issue](https://github.com/eiel/flaun/issues)などに書いていただければ対応するかもしれません。もちろん Pull Request もお待ちしています。
