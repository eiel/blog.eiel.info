---

title: "funtooメモ grubの設定"
date: 2012-06-09T17:59:00+09:00
comments: true
categories: [funtoo,grub]
---
funtooのメモ

grubの設定は `/etc/boot.conf` を雛形に作成される。
変更後は`boot-update`で `/boot/grub.cfg` を生成してくれます。

新しいkernelを `/boot` に配置したときも自動的に生成された印象があるんだけど、あれはどうなってんだろう。
