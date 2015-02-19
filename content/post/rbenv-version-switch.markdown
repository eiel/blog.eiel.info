---

title: "rbenv で Ruby の version を一時的に切り替え"
date: 2013-04-12T01:25:00+09:00
comments: true
tags: ["ruby"]
---

rbenv で ruby の version を一時的に切り替えたい場合があります。
rbenv は名前のとおり環境変数で挙動を変更することができます。

`RBENV_VERSION` を設定しておけばそのRubyを実行をすることができます。

```bash
$ ruby -v
ruby 2.0.0p0 (2013-02-24 revision 39474) [x86_64-darwin12.2.1]
$ RBENV_VERSION=1.9.3-p0 ruby -v
ruby 1.9.3p0 (2011-10-30 revision 33570) [x86_64-darwin12.3.0]
```

ruby だけでなく gem や gem で installしたコマンドにも有効です。

### おまけ

`rbenv local` でそのプロジェクトで使用するRubyを設定できますが、`.rbenv-version`というファイルを生成します。
まれに、生成したくない場合があります。

例えばホームディレクトリにいるときです。
広い範囲で影響がでます。

`rbenv global` を使えばいいという説もありますが、バックグラウンドで ruby の script が動いていると`必要な gem がない`ということが起きることがあります。

そんなときは `rbenv shell`が使えます。
一時的に ruby の version を固定できます。

```
$ ruby -v
ruby 2.0.0p0 (2013-02-24 revision 39474) [x86_64-darwin12.2.1]
$ rbenv shell 1.9.3-p0
$ ruby -v
ruby 1.9.3p0 (2011-10-30 revision 33570) [x86_64-darwin12.3.0]
$ rbenv shell --unset
$ ruby -v
ruby 2.0.0p0 (2013-02-24 revision 39474) [x86_64-darwin12.2.1]
```

基本的には RBENV_VERSION を再設定しているだけのようです。
