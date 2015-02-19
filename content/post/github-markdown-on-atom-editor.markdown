---

title: "Atom Editor で markdownモードのスニペットの追加"
date: 2014-07-07T15:20:00+09:00
comments: true
categories: ["atom editor","markdown"]
---

[Atom Editor](https://atom.io/)をときどき試している。
markdownモードにスニペットを追加する方法がなかなかわからなくて困ったのでメモ。

最初に指定する文法名がわからなかった。`.source.gfm`を指定すれば良かった。
この `.source.gfm` の簡単な調べ方がよくわからない。

gfm は GitHub Flaver Markdown の略だと思われる。

参考例をあげておきます。

```
'.source.gfm':
  'Sample Snippet Hoge':
    'prefix': 'hoge'
    'body': 'hogehoge'
```

`atom.syntax.grammarsByScopeName` にいろんな名前がとれたけど、全然効率よくない。
