---

title: "Ruyb on Railsにて render を before_filterとかafter_filterで読んだら酷い目にあった"
date: 2012-07-12T15:22:00+09:00
comments: true
categories: [rails]
---
コントローラ内デいろんなところで同じrenderを書いてたのでリファクタリングしようと思った。

そんなわけで `after_filter` を利用して render を呼んでみましたが、actionの処理がすでに終了してるようで、 **renderは一度呼んでるよ!** って怒られました。

仕方なく `before_filter` で呼んだら、action内に入る前にレンダリングしてしまって、**@hogehoge がnilやねーんって怒らました。**

とりあえず、あきらめることにしました。
