---

title: "Mac で rbenv 使って ruby-2.0.0-preview1 インストールすると OpenSSLがうごかないのでなんとかしてみた"
date: 2012-11-25T12:08:00+09:00
comments: true
tags: ["rbenv","ruby"]
---

[以前書いたruby-2.0.0をビルドしてみた on Rbenv](blog/2012/11/07/ruby2/)の方法で build できるのですが Mac OSX でやると OpenSSLのバージョンが古いようで、 `bundle install` などが失敗してしまいます。

なので OpenSSLを一緒にインストールするようにパッチを書いてみました。
[github](https://github.com/eiel/ruby-build)にupしてます。

上記の記事と同じ状況であれば以下の操作でインストールできます。
```bash
$ cd ~/.rbenv/plungin/ruby-build
$ git remote add eiel git@github.com:eiel/ruby-build.git
$ git remote update
$ git checkout eiel/master -b eiel
$ rbenv install 2.0.0-preview1
```

patchの内容ですが `share/ruby-build/` にビルド時のルールを定義するファイルがあるのでそこに OpenSSL を追加しました。でもそのままだと失敗したので、`configure` のoptionを追加したり make のオプションを潰したりしてます。

homebrewを使った場合の情報はおちてるんですが Gentoo Prefix を使う身としては使わずになんとかしたかった。

### 参考文献

[http://blog.takuyan.com/blog/2012/11/21/rbenv-install-2-0-0-preview1-and-openssl/](http://blog.takuyan.com/blog/2012/11/21/rbenv-install-2-0-0-preview1-and-openssl/)
[https://github.com/mxcl/homebrew/blob/master/Library/Formula/openssl.rb](https://github.com/mxcl/homebrew/blob/master/Library/Formula/openssl.rb)
