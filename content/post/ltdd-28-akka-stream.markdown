---
date: 2016-08-08T22:18:07+09:00
title: "Akka StreamのDelayで学ぶback-pressure という話をした - #LT駆動"
---

[LT駆動開発28](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA28)でLTをした。

[Scala関西Summit 2016](http://summit.scala-kansai.org/)でAkka Stream系の話をしようとおもっていたけど、結局応募に間に合わせることができなくて、ストックされていたネタを開放しはじめました。
とういうことで、ちょいちょいAkkaやPlay Frameworkの話をその辺でするかもしれません。

本題です。Akka Streamしはじめたときに、Delayでの動作がback-pressureされてることがわかりやすかったので、今回はその話をしました。

<script async class="speakerdeck-embed" data-id="3f590e2cbdc5407896e1d03d874f4a23" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

スライド内にあるコードの詳細はGistにアップしています。

https://gist.github.com/eiel/a8edc165ad316868c75766a447d57237
