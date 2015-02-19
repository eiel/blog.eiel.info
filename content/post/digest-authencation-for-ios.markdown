---

title: "iOS で Digest認証してみる。 - AFNetworking"
date: 2013-03-20T02:20:00+09:00
comments: true
categories: ["iOS","ruby","rack"]
---

iOS で Digest認証するコードを書きました。

サンプルコードの作成は頼まれて作成しただけです。
折角なので、簡単な説明を 記事にしておきます。

   [サンプルコードはこちら](https://github.com/eiel/Digest_Sample)

## テストサーバ構築

まずは動作確認をできるようにしないといけないので Digest認証 をするためのウェブサーバがないと困ります。Ruby の rack を使いました。

Digest認証をするには Rack のミドルウェア `Rack::Auth::Digest::MD5` を使用しました。
Digest認証は ハッシュ化アルゴリズムの選択できるようになっているので、ミドルウェアはこのような名前になっているようです。
Rackのソースコードみにいったら、最初気づかなくて困りました。

`config.ru` は以下のように書きました。

```ruby
use Rack::Auth::Digest::MD5, "auth", '' do |username|
  "password"
end

run proc { [200, {'Content-Type' => 'text/html'}, ['hoge']] }
```

use の第三引数は opaque となります。デフォルトでは `nil` で、設定しないと動きません。はまりました。

蛇足ですが、 `use` の仕組みをよくしらなかったので[ソースコード](https://github.com/rack/rack/blob/1.5.2/lib/rack/builder.rb#L81-L87)をちら見しました。
`lib/builder.rb` に実装があります。

```ruby
    def use(middleware, *args, &block)
      if @map
        mapping, @map = @map, nil
        @use << proc { |app| generate_map app, mapping }
      end
      @use << proc { |app| middleware.new(app, *args, &block) }
    end
```

@map が `nil` の場合は middleware.new する処理が割り込むだけですね。引数はまるまる渡しています。

use する時の引数は 使用するミドルウェア の initialize メソッドをみればよいことがわかります。
Digest認証の場合は ([ソースコード](https://github.com/rack/rack/blob/rack-1.5/lib/rack/auth/digest/md5.rb#L24-L31))

```ruby
        def initialize(app, realm=nil, opaque=nil, &authenticator)
          @passwords_hashed = nil
          if opaque.nil? and realm.respond_to? :values_at
            realm, opaque, @passwords_hashed = realm.values_at :realm, :opaque, :passwords_hashed
          end
          super(app, realm, &authenticator)
          @opaque = opaque
        end
```
第2引数、第3引数、ブロックを用意すればよさそうです。rdoc にも書いてありますが、block は username を引数にうけとります。
確認しないと心配なら [valid_digest?](https://github.com/rack/rack/blob/rack-1.5/lib/rack/auth/digest/md5.rb#L97-L100) で利用されています。

```ruby
        def valid_digest?(auth)
          pw = @authenticator.call(auth.username)
          pw && digest(auth, pw) == auth.response
        end
```

## iOS側

使い方は サンプルコードの [README.md](https://github.com/eiel/Digest_Sample) にかいています。

AFNetworking は iOS と Mac OS X で利用できるライブラリで block や NSOperation を利用して ネットワークの処理がかけるライブラリみたいです。

Digjest 認証をしたい場合は `AFHTTPRequestOperation#setAuthenticationChallengeBlock に認証が必要なときに呼び出される処理を block で登録しておけばよいようです。

なので、あらかじめ登録しておいた username と password を利用して、
認証する処理を登録した状態の AFHTTPRequestOperation を返すようなファクトリを用意してやりました。

あと雑談ですが、

block 内で `self` を利用したい場合は JavaScript の影響で `that` にして使っています。
一般的な変数名はなんなのでしょうか。


## 参考リンク

* [Rack::Auth::Digest::MD5 のつかいかた](http://d.hatena.ne.jp/dayflower/20120711/1342058487)
* [rack](https://github.com/rack/rack)
* [AFNetworking](https://github.com/AFNetworking/AFNetworking)
* [NSURLConnectionクラスを使用したダイジェスト認証処理](http://ch3cooh.hatenablog.jp/entry/20110513/1305264939) 
