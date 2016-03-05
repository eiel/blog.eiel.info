---
date: 2016-03-05T13:26:16+09:00
title: Hiroshima Ruby Conference 2016でしかもオープンデータデイなのでRDFの話をした
tags: ["ruby","rdf","opendata"]
---

[Hiroshima.rb](http://hiroshimarb.github.io/)と[Ruby Association](http://www.ruby.or.jp/ja/)の共催で[Hiroshima Ruby Conference](http://hiroshima-ruby-conf.me/)が開催されました。


この日は[インターナショナルオープンデータデイ](http://okfn.jp/2015/09/16/iodd2016-pre/)でもありオープンデータに関する話をすることにした。

ということで、「Hiroshima.rbの情報をRDFでオープンでリンクなデータにしたんよ」という話をした。

プログラマとしての視点で、RDFについて知りたかったことはQiitaにまとめておきました。

* [RDFに関する雑な説明 - Qiita](http://qiita.com/eielh/items/deadb765ac994956f8a2)

つづいて、RubyでRDFを作る方法をQiitaにまとめておきました。

* [RubyでRDFを構築してみる - Qiita](http://qiita.com/eielh/items/1ae9ef9ae822e4256d52#_reference-e5d1f38862d0df2ef167)

そして、今回は[Hiroshima.rb](http://hiroshimarb.github.io/)のサイトにJSON-LDでエンコーディングして、埋め込みしました。
scriptタグを利用して埋め込みをしています。(詳細はHTMLをみてください)
schemaには https://schema.org/ を利用してるのでGoogleが認識することができます。
RDFのライブラリを使えばウェブページを読み込みして、データを取り出すことが単にできるとおもいます。

RDFにするとデータをつなぐことができて分散データベースをつくることができるというわけです。
RDFはとても柔軟なフォーマットでRDFからいろんな形式をデータを生成できます。
ただし、柔軟性をを得たトレードオフとして処理効率が犠牲になります。
RDFのデータはクエリをつかって、表データのようにデータをとりだせるので、うまく使えばネットで情報を収集して、扱いやすい形式に落として別のシステムへ渡したりもできるとおもいます。

とまあ、今回はRDFについて調べて疑問点だったことを自分が知りたかったことベースにまとめたついでに実践してみたという話でした。
具体例として参考になると幸いです。

<script async class="speakerdeck-embed" data-id="8141be0553cb4fc8960e5e4a30967ce2" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
