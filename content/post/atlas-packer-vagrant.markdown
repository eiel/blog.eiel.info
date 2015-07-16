---
date: 2015-07-16T23:14:57+09:00
title: "AtlasでPackerが実行できるようになってて感動した"
tags: ["packer","vagrant","すごい広島"]
---

[すごい広島113](http://great-h.github.io/events/event-113.html)で遊んでたこと。

テスト環境を提供しないといけなくて、Dockerつかいたかったけど、今回はVagrantのBoxでベースを提供することにした。
Packerつかうと時間がかかるのでvagrant packageでつくって、Vagrant Cloudにおいたりしてました。

ちょっと久しぶりにPackerでboxをつくるかーって[Atlas](https://atlas.hashicorp.com/)にいってみたら、Web上に表示されたチュートリアルにそってコマンドラインで作業するだけでBoxがつくれるようになってました。
しかも、Packerの実行をAtlasがやってくれます。しかも、VirtualboxとVMwareのイメージ両方つくってくれます。
しかも、勝手にBoxesに登録されます。
素敵です。

1. Atlasにサインインする
1. `Build Vagrant Boxes with Packer and Atlas`というメニューがあってクリックする
1. packerのバージョンの確認をさせられる。0.8.0より最新かどうか。
1. PackeのTemplaterをもってるかきかれます。もってなかったら [atlas-packer-vagrant-tutorial](https://github.com/hashicorp/atlas-packer-vagrant-tutorial)が使えると教えてくれます。
1. API TOKENが生成されるので、環境変数に登録します
1. packer push をつかって template.jsonとscriptをAtlasに送信します
1. 進行状況をAtlasで閲覧できます。だいたい30分ぐらいかかります。

そうすると、boxができて、Atlasから取得できるようになります。
ここから配布していると vagrant box update でboxが更新できたりとなかなか嬉しいです。

### 他に遊んだこと

tutorialのものはUbuntu12.04だったので[14.04](https://github.com/eiel/atlas-packer-vagrant-rails/commit/03d5c05b48ec02bae24bbf58d8ebd4f7c380762d)にかえたりしました。

railsが実行できる環境つくって遊びました。
[mysqlとrubyをいれてるだけです](https://github.com/eiel/atlas-packer-vagrant-rails/commit/054deb74f7d6ab4a034304f3f78deee1df386bd5)。2.1.6なのは大人の事情です。

公開するboxは一度公開したバージョンの場合、最終的なPackerの実行が失敗します。
成果物が登録できないからです。
メタデータを修正して、新しいバージョンに変えるようにしてください。

具体的には[この辺を参考にしてください](https://github.com/eiel/atlas-packer-vagrant-rails/commit/683741aa9f6055e70c0822bf2520d4b59ecaca4f)。versionを0.0.4にしています。

[実際につくったVagarantのBoxはこちらにあります。](https://atlas.hashicorp.com/eiel/boxes/rails-mysql)

```
vagrant init eiel/rails-ymqsl
```

とかすると使えます。

Vagrantのboxをつくるのに、Packerをつかうとローカルマシンが占有されてつらかったのですが、Atlas上でビルドできるようになってたいへん幸せです。

継続的デリバリー感がします。とても楽しいので一度遊んでみてください。
