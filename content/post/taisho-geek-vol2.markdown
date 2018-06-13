---
title: "FlowとTypeScriptを同時に使う話を大正GeekNight Vol.2でしました"
date: 2018-06-13T22:18:48+09:00
---

[大正GeekNight Vol.2](https://taisho-geek.connpass.com/event/89869/
で「静的型なきJS界を救う英雄たちの話」というタイトルでLTをしました。

実は会場に行ってなくて、LTしてる動画を録画して送りつけるという参加方式をとりました。
なぜ、会場に行かなかったかというと、私は介護者と共に行動する必要があり、イベントの時間は子供を寝かせる時間帯で、子供も連れてくるという選択肢以外には、録画かリモートで発表する必要がありるためです。
連れて行くという選択肢はありえないし、寝かせるタイミンで配信なんてできるわけがないのでがんばって録画しました。録画するタイミングもあんまりなくて大変だった。動画は倉庫に封印しています。
中身がないので、公開する予定は今のところないですが、希望があれば検討するかもしれません。(もしくはリベンジ)

<script async class="speakerdeck-embed" data-id="33f808eb8ebf43509b02b08ead1f6ce4" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

サンプルプロジェクトはこの辺に投げてあります。
https://github.com/eiel-sample-code/flow-with-typescript

さて、本題ですが、身近なエンジニアがTypeScriptを始めた的な発言をしていたらFlowもどうですか?と聞いてみる遊びをしていたところ、同時に使ったらどうなるんだろうと気になったので、やってみたらできたというのが発端です。

といっても、TypeScriptはちょっとしかつかったことがなかったので、どんなことができそうか判断するために、[FlowのドキュメントにあるコードをひたすらTypeScriptのPlaygroundで試す](https://qiita.com/eielh/items/21168709edf2813acdcc)という遊びをしてみました。
その結果、ファイル分割していくとimportがつらそうだなとわかったので、importをつかいつつ両方でエラーできないコードと、FlowとTypeScriptでエラーがでるのが違うところになるようなサンプルコードをかいて、簡単に解説するという話になりました。

紹介した内容は、

* Maybeがないので`Hoge | null`というnullをくっつけたUnion TypeをつかってMaybeの代用してみた
* Object Typeの挙動の違い

になります。

ところで、このサンプルコード大失敗があって、`noImplicitAny`ぐらいしかtrueにしていないというミスがありました。
https://github.com/eiel-sample-code/flow-with-typescript/blob/master/tsconfig.json#L4

スライド上にあるFlowだけエラーになるコードはTypeScriptでも`strictNullCheck`をつけるとエラーになります。
TypeScriptの評判を落とすような誤解を招く可能性が多いにあり大変申し訳ないです。


蛇足ですが、個人的にはObject Typeの挙動が結構好きなので、Flowを使うのが楽しいです。

というわけで、あまり詳しくないものを活用して、思いつきでLTする場合は、有識者のレビューをいれるとより良いものになると思いました。

その他のFlowとTypeScriptの違いに興味があれば、[FlowのドキュメントにあるコードをひたすらTypeScriptのPlaygroundで試す](https://qiita.com/eielh/items/21168709edf2813acdcc)など参考にしてみたりしてください。
