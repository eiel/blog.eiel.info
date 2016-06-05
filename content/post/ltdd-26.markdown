---
date: 2016-06-05T20:59:52+09:00
title: "「プリエンプティブルVMをずっと起動してみた」という話をした #LT駆動 26"
---

[LT駆動 26](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA26)でLTをしました。

最近Google Compute EngineのプリエンプティブルVMで節約しつつ使えるリソースを増やしていこうと遊んでいます。そんなに増やしてもやることはまだみつけていないけど。


<script async class="speakerdeck-embed" data-id="5bd879ad1ae443f4bab2c7efd242d7ac" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

Google Compute Engineには70%OFFで使えるプリエンプティブルVMというのがあるみたいです。
しかし、24時間しか使えないみたいです。
Google Compute EngineはMetadataのkeyでstartup-scriptにいれておくと起動時に実行してくれるそうです。
shutdown-scriptもあるようです。
また、インスタンスグループは指定した数のインスタンスを維持するようにインスタンスを起動してくれるみたいです。
あとは正常に終了処理ができれば、最低１日１回5分程度落ちていることがあるけど、とても安くつかえそうです。

というわけで、最近つけっぱなしにして様子をみて遊んでいます。

１台じゃ回しきれなくなったらクラスタを組んでロードバランサーも使っていくことで、１台落ちても大丈夫な状態はつくれるんじゃないかと思っていたりもします。

落ちる前提でサーバーを管理するのも結構楽しそうです。
