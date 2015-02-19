---

title: "LT駆動開発で hubot をつかったオフィスに誰がいるかわかるコマンドを作成した話をした"
date: 2014-09-06T19:43:00+09:00
comments: true
tags: ["LT駆動", "slack", "hubot"]
---

[LT駆動開発07](https://github.com/LTDD/Sessions/wiki) でLTしてきた。

今回は hubot をつかってオフィスに誰がいるのかわかるようにした。
どうしてそんなことをしようとするのか、そして簡単に仕組みを紹介しました。

<script async class="speakerdeck-embed" data-id="78832e3017df013292f406657be3bf12" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

みんなが積極的に関わろうとするにはどうするのか。
おもしろかったり便利だと思うことをやること。
興味をもってもらうことだ。

オフィスには人がいたりいなかったりで、「誰かいたらいこうかなー」とか思うこともあるので完全に俺得である。

スライド内のデモ動画はこちらにあります。

<iframe width="420" height="315" src="//www.youtube.com/embed/zb9Y-ZjCW2c?rel=0" frameborder="0" allowfullscreen></iframe>

結構みんなつかっていて、コンボも流行っている。
そのコマンドは`突然のおるんか`とかですね。

デモにいれておけばよかった。

仕組みはMACアドレスをつかっていて、みんなノートPCなのでなかなかうまくいっている。
最初はデーモンをつくろうかと思っていたけど、なくても解決できそうなのでこうなった。

おまけで、他につくったコマンドを紹介しとく。

<iframe width="560" height="315" src="//www.youtube.com/embed/eNSgpdIUfVo?rel=0" frameborder="0" allowfullscreen></iframe>

アリア社長とアリシアさんの画像は[こちらから拝借](http://www.moaibu.com/sozai/aria/index.htm)しております。
ありがとうございます。

非常に癒される。

## おまけ

LT駆動では予備のスライドを用意しておくことが大事らしい。

<script async class="speakerdeck-embed" data-id="c84e2e2017de013292f306657be3bf12" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
