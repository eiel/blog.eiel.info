---

title: "pandocのインストールに失敗する"
date: 2014-01-06T15:45:00+09:00
comments: true
tags: ["hakyll","pandoc"]
---

[hakyll](http://jaspervdj.be/hakyll/) のアップデートしてたら pandoc のビルドにこけた。

エラーの内容は下記のとおり。

```
src/Text/Pandoc/Readers/Haddock/Lex.x:149:46:
    Couldn't match type `(AlexPosn, Char, String)'
                  with `(AlexPosn, t0, t1, [Char])'
    Expected type: (AlexPosn, t0, t1, [Char])
      Actual type: AlexInput
    In the first argument of `go', namely inp'
    In the expression: go inp' sc
    In a case alternative: AlexSkip inp' len -> go inp' sc
```

ぐぐったら以下が出てきた

https://github.com/jgm/pandoc/issues/815

というわけで下記を実行してみた。

```
cabal install alex
```

再度 install してみたら成功した。

失敗したファイルが `*.x` なファイルだけどコンパイル前に alex で処理されるんだろうか…よくわからない。そのうち調べたい。
