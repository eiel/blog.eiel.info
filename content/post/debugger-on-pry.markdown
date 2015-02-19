---

title: "ruby-debugからpryを起動する"
date: 2012-07-05T14:44:00+09:00
comments: true
tags: [ruby,debug,pry]
---
[Pry](https://github.com/pry/pry)便利です。

スクリプト上で debugger をかいておくとそこでデバッガ(rdb)を起動できますが、`debugger-pry`をインストールしておくと`pry`コマンドが追加されて`pry`を起動できます。
Gemfileに書く場合は

``` ruby
gem "debugger-pry", :require => "debugger/pry"
```

最近のRailsのGemfile

{% gist 3051612 %}
