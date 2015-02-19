---

title: "i18n で ActiveModelを使ったモデルの日本語化"
date: 2013-04-29T13:53:00+09:00
comments: true
tags: [active_model, i18n]
---

`ActiveModel` を利用したモデルで i18n を利用しようとして以下のようなyamlを書いたら日本語にならなかった。

```yaml
activerecord:
  models:
    page: "ページ"
```

ソースコードを読んでみた結果、

```yaml
activemodel:
  models:
    page: "ページ"
```

と、書けばいいことがわかりました。

「それはそうですね。」という結果でなんとも言い難いです。
