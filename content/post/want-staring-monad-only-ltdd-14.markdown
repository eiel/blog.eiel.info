---
date: 2015-05-02T13:00:00+09:00
title: モナド則だけ見つめていたい - LT駆動開発14
tags: ["Haskell","LT駆動","モナド"]
---

[LT駆動開発14](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA14)に参加した。

[ゼノブレイドクロス](http://xenobladex.jp/)発売記念でモナドの話をしといた。

<script async class="speakerdeck-embed" data-id="b016eb833b804fca903db71dc869bae0" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

Stateモナドを簡約して、Stateモナドを説明しようとおもったけどうまくいかなくてボツになりました。

* [Haskell - Stateモナドを手で簡約してみたりしていた - Qiita](http://qiita.com/eielh/items/3612e4233c9c4a2d80a0)

そんなわけで[Haskell/圏論 - Wikibooks](http://ja.wikibooks.org/wiki/Haskell/%E5%9C%8F%E8%AB%96)を元ネタにモナド則を辿ってみました。

`a -> M b`って型の関数を並べるにはfmapしてjoinしてを間にはさむことがポイントな気がしたことがあったのでその話です。
`a -> M b`な関数を組み合わせると `M b -> M (M c)` になって `M (M c) -> M (M (M d))` とどんどんMが増えていってしまうのですが、モナドであれば`M d`にできるわけです。

`a -> M b`ってなんなんだって話になってきますが`M a -> M b`でも良いけど、`a -> M b` のほうがあつかいやすいよね。だって`M`は外せないんだから外れているものが受け取れたら便利じゃないですか。
結果的に残ったものは **何度も同じことをしないといけない部分**を隠すことができます。
その内容を自由に取り替えできちゃうのがモナドの魅力なのだと思う。

そしてMに関する操作は裏でひそかに行われて、命令書を構築したり、失敗していたら何もしなかったり、可能性すべてを記録したり、単に設定した値をおけるだけだったり、するだけだと思われます。

### 関連

[Haskell - Stateモナドを手で簡約してみたりしていた - Qiita](http://qiita.com/eielh/items/3612e4233c9c4a2d80a0)

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=eiel-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=B00T73HQHQ" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
