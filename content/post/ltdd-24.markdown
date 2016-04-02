---
date: 2016-04-02T15:00:00+09:00
title: "bluemixで遊ぶならメモリの使用料が少ない言語つかいたいよね、という話をした - #LT駆動 24"
tags: ["LT駆動", "rust", "bluemix"]
---

[LT駆動開発 24](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA24)に参加しました。

bluemixはメモリ使用量で、課金されるので、メモリ消費の少ない言語が有利なんじゃないかと思いて、さまざまな言語で使用メモリを確認をした話をした。
全部はみていないがbluemixで提供されているものでは、Goが良さそうである。
GoならGAE/Goもあるし、Goは幅広くつかえますね。

個人的には、haskellやrustが気になるところである。リッチだし、特にRustはローカルで動作させてみたところ使用メモリがすくなかった。デプロイするには[binary-buildpack](https://github.com/cloudfoundry/binary-buildpack)を使うのが楽そうだけど動かすところまでやってない。
普通にbuildpackをつかってやるとbuild時間がなかなかつらそうです。(heroku-buildpack-rustは見事に失敗してまった)

メモリ使用量のところを抜粋しときます。
<table>
<tr>
<td>go</td><td>7MB</td>
</tr>
<tr>
<td>swift</td><td>14MB</td>
</tr>
<tr>
<td>python</td><td>40MB</td>
</tr>
<tr>
<td>php</td><td>57MB</td>
</tr>
<tr>
<td>nodejs</td><td>74MB</td>
</tr>
<tr>
<td>ruby</td><td>75MB</td>
</tr>
<tr>
<td>.net</td><td>152MB</td>
</tr>
<tr>
<td>java</td><td>177MB</td>
</tr>
</table>

<script async class="speakerdeck-embed" data-id="24768e8433e14c479bd6f32a2999caa8" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
