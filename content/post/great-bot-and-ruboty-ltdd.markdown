---

title: "LT駆動開発で ruboty の使い方っぽいことを適当に紹介した"
date: 2014-07-07T17:14:00+09:00
comments: true
categories: ["ltdd","ruboty","すごい広島"]
---

[LT駆動開発 05](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA05) でライトニングトークをした。

6月は[すごい広島のBOT](https://twitter.com/great_hiroshima)を作って遊ぶことをしていた気がしたのでこれを紹介することにした。これは [Ruboty](https://github.com/r7kamura/ruboty) で作成した。

何か毎回やってないネタを挟みたいということで、ふたつスライドを用意して相互参照するというネタをした。
まずは、BOTの具体例である「すごい広島 BOT」を紹介することでなにがしたいのかをBOTを知らない人に理解してもらう。
そこから、どうやってそれを作るのか。というところに焦点をおいたというライトニングトークするという流れである。

しかし、実際、ふたつもライトニングトークをする時間があるかどうかわからないので「Rubotyの使い方」はかなり適当につくってしまった。
類似性を持たせるために、前半は同じ構成になっている。

最終的にしたいことは、「すごい広島のTwitterアカウントを誰か運用してくれ」ということかもしれない。

<script async class="speakerdeck-embed" data-id="589b86f0e4c8013160d4220f151a2dd2" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

<script async class="speakerdeck-embed" data-id="22966d40e4c90131438f225375d05812" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

すごい広島のBOTは今後もプロキシーBOTとして活躍していただきたいところである。

[ruboty-twitter](https://github.com/r7kamura/ruboty-twitter/) の fork をみてみると、デバッグ用のコード仕込んでみたりとかしているところがあって、うまくメンションが返ってこなかったからかと思い、その辺を補足してみたという経緯があったりなかったりもします。

### 関連

* [Rackup で Ruboty](http://blog.eiel.info/blog/2014/06/08/ruboty-with-web/)
* [すごい広島 一周年](http://blog.eiel.info/blog/2014/05/23/1st-anniversary-for-great-h/)
* [LT駆動開発という勉強会をはじめるよ](http://blog.eiel.info/blog/2014/02/19/start-ltdd/)
