---

title: "redmine-1.4系に更新してruby1.9で動かそうとしてはまったこと"
date: 2012-05-10T17:00:00+09:00
comments: true
tags: [redmine]
---
mysqlがよめないっていわれた。

> cannot load such file -- mysql (MissingSourceFile)

bundle installすると mysql2がはいるのであたりまえなんだけど、なんでかなー。
と考えた結果 `config/database.yml`でadapterを`mysql`にしてるからだ！と気づいて`mysql2`にかきかえました。
