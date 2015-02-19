---

title: "最近のMacでRubyのビルドに失敗するなら brew unlink apple-gcc42とかしたらいいかもしれない"
date: 2014-10-14T16:03:00+09:00
comments: true
categories: ["Ruby"]
---

友人のMacでRubyのインストールに失敗するらしいということでログとかいろいろみせてもらった。

結果だけいうと

```
$ brew unlink apple-gcc42
```

を実行してもらったらなおった。

### もっと詳しく

rbenv と ruby-build をつかっているっぽくてログファイルを渡してもらった。
ログは失敗したときにここにあるよ。的なのが端末にでているはずだ。

たぶん、`/var/folders/XXXXXXXXXXXXXXXXXX/ruby-build.20141014143529.76588.log`みたいな感じになっている。
それを詳しい人に渡してきくようにするといいと思う。

中身をみたら ./configure で失敗していたので config.log をみせてもらった。

そしたら

```
configure:3064: result: x86_64-apple-darwin13.4.0
configure:3335: checking for gcc-4.2
configure:3351: found /usr/local/bin/gcc-4.2
configure:3362: result: gcc-4.2
```

となってて

```
Target: i686-apple-darwin11
```

となってて

```
ld: library not found for -lcrt1.10.6.o
collect2: ld returned 1 exit status
```

なってた。

ターゲットが `x86_64-apple-darwin13` になってないのがなんか嫌だなと思いつつ、事例が「[rbenvで ruby インストールエラー (OS X mavericks)  - QA@IT](http://qa.atmarkit.co.jp/q/3491)」に似ているので、

```
sudo ln -sf /usr/bin/llvm-gcc-4.2 /usr/bin/gcc-4.2
```

で解決すること的なことが書いてあるけどなんか嫌だなぁってことで、そもそもなんで`/usr/local/bin`に`gcc-4.2`があるねん、ということで

```
ls -l /usr/local/bin
```

してもらったらsymlinkでhomebrewでインストールしたapple-gcc42だったので、

```
brew unlink apple-gcc42
```

してもらったら、うまくいきました。

おわり。

たぶん、一時期の間、apple-gcc42をいれないとrubyがインストールできなかった時代があるのが原因と予想される。

* 参考 [Cannot install ruby-1.9.2 in Mac OSX 10.8.1 due to symlink error - Stack Overflow](http://stackoverflow.com/questions/12578220/cannot-install-ruby-1-9-2-in-mac-osx-10-8-1-due-to-symlink-error)
