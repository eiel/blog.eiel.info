---

title: "LT駆動開発10でTest KitchenではじめるChef入門という話をした。OSH2015のステマかもしれない。"
date: 2015-01-10T12:00:00+09:00
comments: true
tags: [osh2015,chef]
---

[LT駆動開発10](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA10)で「Test KitchenではじめるChef入門」という話をした。

ちなみにこの記事は[オープンセミナー2015@広島](http://osh-web.github.io/2015/)のステマ記事でもある。

また、[「Test KitchenではじめるChef入門」はQiitaに投稿した記事である](http://qiita.com/eielh/items/64e197f4f1eaf5ff6097)。


<script async class="speakerdeck-embed" data-id="ba236a70755b0132ffbd4aca611a7ac2" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

Chefはちょいちょい使っていて、使いこなせてはいません。

Cookbookを作成するときの環境で、Vagrantfileでやるのがよいのか、knife-zeroを使うのがいいのかいろいろ悩んでいた。
やっぱりテストコードを書きたいし、ますは[Test Kitchen](https://github.com/test-kitchen/test-kitchen)をいじっておこうと至るのは自然な道だと思う。

Test Kitchenをいじっていると、そのままの環境でCookbookを書くのが良さそうだということに気づいた。
ならば、はじめからTest Kitchenを使いつつChefを学ぶのも良さそうだということで「Test KitchenではじめるChef入門」というネタを考えた。

詳細はQiitaの記事を参照して欲しい。

広島のみんなは[オープンセミナー2014@広島](http://osh-2014.github.io/)で[@t_wada](https://twitter.com/t_wada)さんに来てもらったことだし、何をはじめるにしてもテストコードをかく環境から導入したいはずだ。
きっと、Chefのテスト環境にみんな興味があるはずだ。(確信)(そんなわけはない)

そういえば、[オープンセミナー2015@広島](http://osh-web.github.io/2015/)は2月14日が行なわれる。

[事前申し込みはこちらからできる](http://osh-web.doorkeeper.jp/events/18561)。

そして、みんなすごいChefを試していると思うんだけど、Chefの調べものをしていると、きっと[@sawanoboly](https://twitter.com/sawanoboly)さんを何度もみかけるはずだ。
代表的な記事をピックアップしてみよう。

* [Ruby - Chefのローカルモードチュートリアル + knife-zero + knife-sakura - Qiita](http://qiita.com/sawanoboly/items/4f363909615d8a76e9e5)
* [test-kitchenのつかいかた - Qiita](http://qiita.com/sawanoboly/items/9f560bd63ad0712b17ba)
* [Chefのローカルモードだけでリモートサーバを運用してみようと、Knife-Zeroを作った。Nodeの構成情報もとれるよ。 - Qiita](http://qiita.com/sawanoboly/items/218a7b03ddec6be45e34)
ところで、オープンセミナー2015@広島の講師に[@sawanoboly](https://twitter.com/sawanoboly)さんがいるように見えるのは僕の気のせいだろか。

Chefに限らず構成管理ツールの利用箇所は幅が広がりつつある。

* Packerをつかって各仮想環境のbox作成
* Vagrantのプロビション
* Dockerなどコンテナのプロビジョン
* WerkerのBox作成
* GitLabのインストーラ

あまり知らないけど、僕が知る限りでも、そこそこある。
もっとあるんじゃないかと思う。
構成管理ツールは運用する人たちののツールから開発をする人たちでも使われていくツールなるだろう。

せっかく[@sawanoboly](https://twitter.com/sawanoboly)さんがいらいしゃるので、いまのうちにChefをしっかりいじっておきたいよね。

他にも広島でChefをいじっている人いえば、[@akira345](https://twitter.com/akira345)さんとか、[@k2works](https://twitter.com/k2works)さんとか、[@ogatomo](https://twitter.com/ogatomo)さんとか、[@moobay9](https://twitter.com/moobay)さんとか、[@pecosantoyobe](https://twitter.com/pecosantoyobe)さんとかがいる。
Ansibleなら[@soudai](https://twitter.com/soudai1025)さんとか[@24motz](https://twitter.com/24motz)さんとか、[@yukilab](https://twitter.com/yukilab)さんとか、[@NeXTSTEP2OSX](https://twitter.com/NeXTSTEP2OSX)さんとかがいじっていると聞いている。

たぶんTwitter IDを上げた人はほとんど参加するはずだ。間違いない。
名前を上げなかった人でもきっと何かしらの構成管理ツールを使っている人がたくさんいて遊びに来るはずだ。
[@sawanoboly](https://twitter.com/sawanoboly)さんが来るんだし、きっと間違いない。(自信なし)

さあ、みんなでオープンセミナー2015@広島に集まって情報交換しよう。
構成管理に入門するタイミングとしては、この上はないはずだ。

そういえば以前こんな記事を書いている人がいましたね。

* [オープンセミナー広島は広島のITエンジニアが集う場所 - そんなこと覚えてない](http://blog.eiel.info/blog/2014/01/04/lets-take-part-in-osh/)

くりかえす。

[オープンセミナー2015@広島](http://osh-web.github.io/2015/)は2月14日だ。

[事前申し込みはこちらからできる](http://osh-web.doorkeeper.jp/events/18561)ぞい。

全く関係ないし、公式に受理されていないが、オープンセミナー2015@広島の懇親会の裏の名前はLT駆動開発11なる予定だ。

# 関連

* [JunkBox～主に個人的防備録～: オープンセミナー2015@広島を開催します！](http://akira-junkbox.blogspot.jp/2014/12/2015.html)
