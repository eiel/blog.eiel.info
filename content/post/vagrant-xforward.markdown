---

title: "VagrantでX転送"
date: 2014-12-10T20:47:00+09:00
comments: true
tags: ["vagrant"]
---

以下をかいておけばよい。

```
config.ssh.forward_x11 = true
```

あとは普通に`vagrant ssh`して、Xクライアントを動かせばよい。

サーバ側にある程度必要なライブラリがない場合はディスプレイがないと言われてしまう。
CentOSなら以下をいれとくとよい。

```
yum -y groupinstall "X Window System" vlgothic-fonts

```
