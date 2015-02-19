---

title: "さくらVPSでGentoo をインストールした時のはまったことのメモ"
date: 2014-07-01T17:58:00+09:00
comments: true
categories: ["gentoo","linux"]
---

つかってた VPS がサービス終了してしまったので、さくら VPS に移行してみた。
折角なので OS は [Gentoo Linux](https://www.gentoo.org/) を選択した。

前の VPS では Debian をつかってた。
なんか、ウェブサーバの応答がよくなった気がする。

一応はまったことを書いておくけど大したことは書いていない。

iso アップロードして、普通にインストールした。

### デバイス eth0 がみつからない

なんか enp0s3 として認識してた。
おかげで、ネットワークつながらなくて苦労した。

* 参考 [さくらのVPS（2G）でのハマリポイント(enp0s3) - /dev/curiosity](http://gakutarou.hatenablog.com/entry/2013/05/22/204507)

### ディスク が vda だった

「ディスクがねぇぇええ」って、デフォのCentOS起動してよくみたら `sda` じゃなくて `vda` だった。

### GPT に挑戦したら GRUB が入らなかった

最近Linuxインストールしてなかった。GPTとやらにしてみることにした。
なにも考えずにデフォのCentOSのパーティションをそのまま利用して構築していた。
さあ、GRUBをインスールするぞ。というところで気づいたのだけど、[BIOS boot partition](http://www.gnu.org/software/grub/manual/html_node/BIOS-installation.html)とやらなく、GRUBのインストールに失敗するので、対処を迫られた。

すでにカーネル配置済みの `/boot` の中身を一旦退避して、パーティションを書き換えてごまかした。

### ブートCD がある方法で、ディスクブートする方法がわからない

15秒放置するだけでした。

### virtio いれわすれてディスクがみつけられない

とりあえず、 genkernel したからなにもいらないかとおもったけどダメだったらしい。

```
$ genkernel --virtio all
```

とか genkernel にいろいろオプションがあるのを学んだ。

### まとめ

Gentoo力上がってるので、あんまりハマってない。
最初のネットワークが繋がらないで挫折しそうだったのは秘密である。

### 他に参考したものとか

* [Ruby x Agile version:β - Rubyとアジャイルでふつうのシステム開発を実現する永和システムマネジメントのWebサイト](http://ruby.agile.esm.co.jp/pages/HowToInstallGentooForSakuraVps)

コンパネにもかいってあったけどシリアルコンソール用の設定とかみないと忘れていたであろう。
