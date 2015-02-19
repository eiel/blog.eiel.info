---

title: "GnuPG で遊ぶ - 暗号化してみる"
date: 2013-07-31T19:26:00+09:00
comments: true
categories: ["gpg"]
---

GnuPG 使う機会が無い。無さすぎるよ。使わないと忘れそうなので、たまには遊ぶ。ついでに広まればいいな。ということで、整理しながら試す。

そうそう、これは [すごい広島 #011](http://great-h.github.io/events/event-011.html) で遊んだことです。

### GPGとは

[GNU Privacy Guard](http://www.gnupg.org/) という暗号化ソフト。ざっくりとは [Wikipedia の GPG](http://ja.wikipedia.org/wiki/GNU_Privacy_Guard)とかみてみると良いかも。

[PGPの実装のひとつ](http://ja.wikipedia.org/wiki/Pretty_Good_Privacy)です。
これを使うと暗号化とか署名とかできる。

* 暗号化すると特定の人しか解読できないファイルが作成できる
* 署名すると特定の人が作成したことを示せる

メールの暗号化にも使える。しかし、私は暗号化されたメールを受信したことがない。

[GnuPGの使い方](http://gnupg.hclippr.com/howto.html) あたりを見ながら復習。

### GPG がインストールされているか確認する

```
$ gpg --version
gpg (GnuPG) 2.0.20
libgcrypt 1.5.3
Copyright (C) 2013 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: xxxx
サポートしているアルゴリズム:
公開鍵: RSA, ELG, DSA
暗号方式: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256,
      TWOFISH, CAMELLIA128, CAMELLIA192, CAMELLIA256
ハッシュ: MD5, SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
圧縮: 無圧縮, ZIP, ZLIB
```

入ってるとこんな感じ。


### 暗号化してみる

暗号化する場合

* 暗号化するための鍵を取得する
* 暗号化する
* 読みたい人に送りつける
* 読む人が自分の鍵で複合する

暗号化するための鍵が必要になります。
暗号化してメールを送って欲しい人は「公開鍵」という種類の鍵を鍵を公開しています。なので、これを取得すれば暗号化できます。

復号化するほうは「公開鍵」とペアになっている「秘密鍵」を使います。
これは復号化する人しか持っていないので、メールの暗号化が成立するわけです。

### 実際にやってみる

実際にやってみたいと思います。

* メインのコンピュータで自分の鍵を作成する
* 自分の公開鍵を公開する
* 別のコンピュータで、公開鍵を取得する
* 別のコンピュータで暗号化したファイルを作成する
* メインのコンピュータにファイルを送る
* メインのコンピュータで復号する

というシナリオでやります。

メインのコンピュータでの実行は `main $` をつけて、別のマシンの場合は `sub $` をつけて明示しておきます。
サブのコンピュータを用意する簡単な方法は仮想マシンでしょうか。


#### メインのコンピュータで自分の鍵を作成する

鍵がないことには始まらないので作成します。`--key-gen` オプションを使用します。

すでに作成しているので、実際には使用しない鍵で例をあげます。

```
main $ gpg --gen-key
ご希望の鍵の種類を選択してください:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (署名のみ)
   (4) RSA (署名のみ)
選択? 1

SA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048)
要求された鍵長は2048ビット
鍵の有効期限を指定してください。
         0 = 鍵は無期限
      <n>  = 鍵は n 日間で満了
      <n>w = 鍵は n 週間で満了
      <n>m = 鍵は n か月間で満了
      <n>y = 鍵は n 年間で満了
鍵の有効期間は? (0)
これで正しいですか? (y/N) y
あなたの鍵を同定するためにユーザーIDが必要です。
このソフトは本名、コメント、電子メール・アドレスから
次の書式でユーザーIDを構成します:
    "Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>"

本名: Tomohiko Himura
電子メール・アドレス: eiel at eiel.info
コメント: eiel
次のユーザーIDを選択しました:
    “Tomohiko Himura (eiel) <eiel at eiel.inof>”
名前(N)、コメント(C)、電子メール(E)の変更、またはOK(O)か終了(Q)? O
秘密鍵を保護するためにパスフレーズがいります。

今から長い乱数を生成します。キーボードを打つとか、マウスを動かす
とか、ディスクにアクセスするとかの他のことをすると、乱数生成子で
乱雑さの大きないい乱数を生成しやすくなるので、お勧めいたします。
......+++++
.+++++
今から長い乱数を生成します。キーボードを打つとか、マウスを動かす
とか、ディスクにアクセスするとかの他のことをすると、乱数生成子で
乱雑さの大きないい乱数を生成しやすくなるので、お勧めいたします。
.+++++
十分な長さの乱数が得られません。OSがもっと乱雑さを収集
できるよう、何かしてください! (あと75バイトいります)
.gpg: 鍵CB547A37を絶対的に信用するよう記録しました
公開鍵と秘密鍵を作成し、署名しました。
```

例に使用したメールアドレスは存在しません。あと `@` を ` at ` に置き換えてます。

作成した「公開鍵」があることを確認します。`--list-keys` オプションを使用します。鍵IDは `CB547A37` になります。


```
main $ gpg --list-keys
pub   2048R/CB547A37 2013-07-31
uid                  Tomohiko Himura (eiel) <eiel@eiel.info>
sub   2048R/C2A0FFF5 2013-07-31
```

秘密鍵があることを確認します。`--list-secret-keys` オプションを使用します。

```
main $ gpg --list-secret-keys
/home/eiel/.gnupg/secring.gpg
-----------------------------
sec   2048R/CB547A37 2013-07-31
uid                  Tomohiko Himura (eiel) <eiel@eiel.info>
ssb   2048R/C2A0FFF5 2013-07-31
```

みたいな出力がでました。

`sec   2048R/CA566D81 2009-11-21`
 というのは 秘密鍵で 2048 の RSA の鍵で、 鍵ID が `CA566D81` というように読めそうです。
公開鍵の場合は pub がついているようです。
ssb や sub はなんなのでしょうか。
どこを見ればわかるのか知りたいです。


#### 自分の公開鍵を公開する

鍵を公開するのには 鍵サーバというものが提供されています。
`--send-keys` オプションを使うと簡単に登録できます。

```
main $ gpg --send-keys CB547A37
```

間違えて送ってしまったものを削除するには `--gen-revoke` で執行証明書を作成して、`revoke.asc` に保存している場合は

```
main $ gpg --send-keys CB547A37 < revoke.asc
```

で削除できました。
正しいやりかたかどうかよくわからないので、ざっくりと書いておきます。

公開されている鍵を探してみます。
相手のメールアドレスがだいたいわかれば検索できます。
以下は以前公開したものを利用してます。
都合によりさきほど作成したものと違います。

```
sub $ gpg --search-keys eiel.hal
gpg: “eiel.hal”をhkpサーバーkeys.gnupg.netから検索
(1)	Tomohiko Himura <eiel.hal@gmail.com>
	Tomohiko Himura <tomohiko.himura@gmail.com>
	  2048 bit RSA key CA566D81, 作成: 2009-11-21
```

`--search-keys` オプションで鍵サーバから検索できます。鍵のID は `CA566D81`です。
検索で見つけられない場合は、Webサイトなどで公開している場合があります。
鍵を見つけたので、鍵を取得します。

```
sub $ gpg --recv-keys CA566D81
```

これで鍵が取得できました。確認してみます。
`--list-keys` オプション` を使用します。

```
pub   2048R/CA566D81 2009-11-21
uid                  Tomohiko Himura <eiel.hal at gmail.com>
uid                  Tomohiko Himura <tomohiko.himura at gmail.com>
sub   2048R/72D733CE 2009-11-21
```

間違えた時のために消す方法も確認します。
`--delete-keys` を使います。

```
sub $ gpg --delete-keys CA566D81
```

#### 別のコンピュータで暗号化したファイルを作成する

暗号化してみます。
まずは、暗号化するファイルを用意します。
`hoge.txt` としたことにします。次に `-e` 使うことで暗号化を示し、`-r` で、どの鍵を使うか設定します。

```
sub $ gpg -e -r CA566D81 hoge.txt
```

とすると、`hoge.txt.gpg` というファイルが作成されました。
中身を見て確認すると暗号されていると思います。

信用できてない鍵を使用している旨が出ます。
鍵の信頼性を上げる方法は宿題にしました。

`-r` には メールアドレスでもOKでした。

```
gpg -e -r eiel.hal@gmail.com hoge.txt
```

#### メインのコンピュータにファイルを送る

なんとかして取得しましょう。
私は scp で取得しました。

#### メインのコンピュータで復号する

復号します。
オプションはいりません。

```
main $ gpg hoge.txt.gpg
```

`hoge.txt` が作成されています。

試しに、別のコンピュータで復号してみました。

```
gpg hoge.txt.gpg
gpg: 2048-ビットRSA鍵, ID 72D733CE, 日付2009-11-21に暗号化されました
      “Tomohiko Himura <eiel.hal@gmail.com>”
gpg: 復号に失敗しました: 秘密鍵が得られません
```

見事に失敗しました。

署名も似たような感じにできると思います。

### 複数人が復号できるファイルを作ってみる

`-r` を複数指定すればできた。

```
$ gpg -e -r メールアドレス -r メールアドレス2 hoge.txt

```

### まとめ


暗号化したいなら相手の公開鍵が必要です。公開鍵を公開しないと暗号メールも届くことはないでしょう。たぶん。

自分で暗号化して、自分だけ復号化したい場合は、ひとつのコンピュータだけでできます。
今回は復号できないのを確認できるように、別々のコンピュータで確認しました。

メールで使う場合はメールクライアントが自動でやってくれるようにできると思います。流れがわかっているほうが安心して使えると思います。
今度は鍵の信頼性を上げる方法や署名の方法を試してみたいです。
