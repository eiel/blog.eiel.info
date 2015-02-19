---

title: "ruby 2.0.0-preview2 をいれて rails起動してみた"
date: 2012-12-10T13:49:00+09:00
comments: true
categories: ["ruby", "rails"]
---

[ruby 2.0.0-preview2](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-core/50443) が出てるのでビルドして rails を起動してみた。

```bash
$ uname -v
Darwin Kernel Version 12.2.1: Thu Oct 18 16:32:48 PDT 2012; root:xnu-2050.20.9~2/RELEASE_X86_64
```

[preview1のとき](/blog/2012/11/25/rbenv-ruby-2-dot-0-0-inclued-openssl/)と同様にopenSSLがついてこないので、OpenSSLを一緒にビルドするようにrbenvを修正しました。[https://github.com/eiel/ruby-build/tree/2.0.0-preview2-with_openssl]

`bundle instal`の実行で

> ~/.rbenv/versions/2.0.0-preview2/lib/ruby/2.0.0/rubygems/core_ext/kernel_require.rb:45:in `require': cannot load such file -- rubygems/format (LoadError)

と出てしまいますが rubygemsのライブラリ構成が変わっててbundlerが動かないだけみたいなので、

```bash
$ gem install bundler --version ">= 1.3.0.pre2"
```

として 1.3 系の gem をいれました。

あとは普通に rails が起動できました。
