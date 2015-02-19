---

title: "cmigemoがインストールできない - openlab.ring.gr.jp が死んでるっぽい"
date: 2014-01-09T02:12:00+09:00
comments: true
categories: ["migemo","skk"]
---

Twitter に書いてもあまり反応がないので、blogにでも書いとくとなんとかなるかもしれないのでかいておくことにする。

[http://openlab.ring.gr.jp/](http://openlab.ring.gr.jp/) が死んでるっぽくて `$ brew install cmigemo` が失敗してしまう。
もしかして ddskk とかも一部インストールできない状態なんだろうか…。ミラーとかないんだろうか。

### 詳細

```
$ brew install cmigemo
==> Downloading http://cmigemo.googlecode.com/files/cmigemo-default-src-20110227
Already downloaded: /Library/Caches/Homebrew/cmigemo-20110227.zip
==> Patching
patching file src/wordbuf.c
==> chmod +x ./configure
==> ./configure --prefix=/usr/local/Cellar/cmigemo/20110227
==> make osx
==> make osx-dict
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
curl: (56) Recv failure: Connection reset by peer
make[2]: *** [SKK-JISYO.L] Error 56
make[1]: *** [dictionary] Error 2
make: *** [osx-dict] Error 2

READ THIS: https://github.com/Homebrew/homebrew/wiki/troubleshooting

These open issues may also help:
    https://github.com/Homebrew/homebrew/pull/7005
    https://github.com/Homebrew/homebrew/pull/15343
    https://github.com/Homebrew/homebrew/issues/10898
```

となる。

`$ brew edit cmigemo` で formula がひらける

make osx-dict に失敗してる

一時ファイルどっかにないんだろうか。

```
$ find /usr/local -name cmigemo\*
/usr/local/bin/cmigemo
/usr/local/Library/Formula/cmigemo.rb
```

特にみつからず。もしや `/tmp/` 以下なんだろうか…と後でおもった。

```
$ wget http://cmigemo.googlecode.com/files/cmigemo-default-src-20110227.zip
$ zip -u cmigemo-default-src-20110227.zip
$ cd cmigemo-default-src
$ chmod +x ./configure
$ ./configure
$ make osx
$ make osx-dict
```

curl -O http://openlab.ring.gr.jp/skk/dic/SKK-JISYO.L.gz
が失敗してることがわかった。

この辺の上手い調べ方とか知りたい。
