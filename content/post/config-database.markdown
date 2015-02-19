---

title: "config/database.yml の情報にアクセスする。"
date: 2013-10-26T00:34:00+09:00
comments: true
tags: [rails]
---

Rails の config/database.yml の情報にアクセスしたかった。

別に open して read して YAML.parse するだけなのですが、
もしかすると、 config/database.yml じゃないところを読むように設定を変えてる場合もありますからね。(ねーよ)

ということで適当にリポジトリを検索したら、

```
Rails.application.config.database_configuration
```

で、取り出せました。
[この辺りはバージョンよって違うかもしれないので、GitHub で検索するのが良いかもしれないです。](https://github.com/rails/rails/search?q=database.yml&ref=cmdform)
