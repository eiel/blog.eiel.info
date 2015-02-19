---

title: "libv8 が コンパイルしたり、バイナリで入ったりで、気になったので少し調べた。"
date: 2013-04-03T19:18:00+09:00
comments: true
categories: ["ruby"]
---

気になったというか、環境によって失敗すると言われてしまった。

libv8 は the ruby racecer のバックエンドです。

調べてみると、「libv8 はバイナリバージョンの gem」 と「ビルドするバージョンの gem」があるのですね。
バイナリバージョンは当然のようにアーキテクチャごとあるようです。

* [libv8のバージョン一覧](https://rubygems.org/gems/libv8/versions)

```
3.16.14.1 March 28, 2013 x86_64-darwin-10
3.16.14.1 March 28, 2013
3.16.14.1 March 28, 2013 x86_64-linux yanked
3.16.14.1 March 28, 2013 x86-linux yanked
3.16.14.0 March 28, 2013
3.15.11.1 January 8, 2013 yanked
3.15.11.0 January 8, 2013 x86_64-linux yanked
3.15.11.0 January 8, 2013 yanked
3.11.8.17 March 22, 2013 x86_64-linux
3.11.8.17 March 22, 2013 x86_64-darwin-12
3.11.8.17 March 22, 2013 x86-linux
3.11.8.17 March 22, 2013
3.11.8.17 March 22, 2013 x86_64-darwin-10
3.11.8.16 March 22, 2013
3.11.8.14 March 22, 2013 yanked
```

ビルドが走ってしまうと、ビルドに失敗する環境や低スペックマシンでつらいので、いろんなバージョンのバイナリのある `3.11.8.13` に固定しました。
`x86_64-darwin-10` があるのに `x86_64-darwin-11` がなかったり少し謎ですが、深追いはしていません。

最新に保つには定期的にチェックするしかないのでしょうか。

libv8 のビルドは最近のマシンでもなかなか長いです。

```
gem 'libv8', '3.11.8.13'
```

rubygems に関する知識が全然足りてないのですが、こういうことができることははじめて知りました。あとで gemspec をチェックしてみたい。


### 参考文献

* [http://d.hatena.ne.jp/suu-g/20121222/1356189597](http://d.hatena.ne.jp/suu-g/20121222/1356189597)
* [https://rubygems.org/gems/libv8/versions](https://rubygems.org/gems/libv8/versions)
