---

title: "Pryでエイリアスを作成する"
date: 2012-08-04T22:03:00+09:00
comments: true
categories: [ruby,pry]
---

[pry](http://pryrepl.org)便利ですね。
`edit-method`をよくつかいます。
ファイル開くためだけに使うときもあります。


だんだん、`edit-method`ってかくのがめんどくさくなってきたので、`em` あたりで利用したくなってきたので、やりかたを調べました。

```
Pry.config.commands.alias_command "em", "edit-method"
```

こんな感じらしいです。第1引数が作成するエイリアス。第2引数が元のコマンドです。

pryの起動時の読み込みファイルは`~/.pryrc`なので、そこにかいてやればOKです。
