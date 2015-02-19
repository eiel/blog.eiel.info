---
title: "Railsによる開発にはzeusが新たな定番になりそう。"
date: 2012-10-13T11:52:00+09:00
comments: true
tags: [Rails,zeus]
---

`rails c` や `rake spec` ってすごく時間がかかる。それをできるだけ早くするプロダクトはいままでにもいろいろありました。`rails-sh`とか`spork`とか。

そんな中[zeus](https://github.com/burke/zeus)というのが最近登場したみたいです。`gem install zeusu` でインストール。`Gemfile`に書いてもいいそうですが、かかなくて良いようです。

あとは RAILS_ROOTで `zeus init` して `zeus start` しておけば、あとは別の端末で `zeus c` や `zeusu s`として使うだけです。ファイルを変更すると認識して再読込します。`zeus init`した際に`zeus.json`が生成されます。
変更箇所に応じて必要なところからforkしなおすような挙動をしているように見えます(よくわかっていません)

cucumberやrspecも認識して `zeusu cucumber` や `zeusu spec` というものも用意してくれます。

まだ使い込んでいませんが、イノベーションを感じるのでしばらく使ってみようと思います。
