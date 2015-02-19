---

title: "広島Ruby勉強会 #32 で 発表したこと - ActiveSupport, jenkins"
date: 2013-07-08T01:00:00+09:00
comments: true
tags: ["ruby","jenkins","activesupport"]
---

[広島Ruby勉強会 #032](http://hiroshimarb.github.io/blog/2013/07/06/hiroshimarb-32/) で、紹介したこととか、喋ったこととかまとめときます。

広島Ruby勉強会の各発表は [Github の Wiki](https://github.com/hiroshimarb/hiroshimarb.github.com/wiki/2013%E5%B9%B47%E6%9C%88%E3%81%AE%E6%B4%BB%E5%8B%95) に整理されてます。

* Rails のソースコード読んでるので面白そうなメソッドを紹介する - ActiveSupport Core Ext
* すごい cron - Jenkins を試した

[勉強会自体の感想は別のところに書きました。](http://eielh-life.tumblr.com/post/54757403133/ruby-032)

## Rails のソースコード読んでるので面白そうなメソッドを紹介する - ActiveSupport

ここ最近は[ほぼ毎日 Rails のソースコードを読んで簡単にメモをとっています。](https://github.com/eiel/railsdoc.eiel.info/commits/master)います。
概ね毎日サボらずやれております。

この内容は [railsdoc.eiel.info](http://railsdoc.eiel.info/) で垂れ流しています。

まずは、ActiveSupport から攻めています。特に Core Ext の部分を読んでいます。ということで、4月から6月の間に読んだものの中で、適当に抜粋して紹介しました。

* [内容はこちら](http://railsdoc.eiel.info/hiroshimarb/32/)

その他には読んでいて気がついたことを残しています。

## すごい cron - Jenkins を試した

ローカルに Jenkins インストールして、これ cron の代わりに使えることに気づいたので、使用してみました。
その中で気づいたことや問題点についてお話をしました。
おまけで ruby 関連の Jenkins の Plugin についてわかったことを話しました。

<iframe src="http://www.slideshare.net/slideshow/embed_code/23971945" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen webkitallowfullscreen mozallowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="http://www.slideshare.net/TomohikoHimura/jenkins-23971945" title="すごい cron ? - Jenkins 試した" target="_blank">すごい cron ? - Jenkins 試した</a> </strong> from <strong><a href="http://www.slideshare.net/TomohikoHimura" target="_blank">Tomohiko Himura</a></strong> </div>

具体的な設定方法は まだ公開してないです。ごめんなさい。
また時間をとって書きたいと思います。

しかし、cron として使うにはメモリを食いすぎるので、Local に Jenkins が欲しくなったら…ぐらいで丁度いいかもしれません。

そもそも Jenkins を導入した動機ですが、実行に10分ぐらいかかるテストがあって、これを独立して実行したかったからです。
あと、失敗したテストの一覧を残しておきかったからです。

というわけで、広島Ruby勉強会 は自由に発表の練習するところになりつつあります。気軽に何か発表しにいきましょう。

次回は [8月3日](http://partake.in/events/9dacdbfc-8acf-4968-a0eb-5327a6937b7d#) だそうです。

## 蛇足

そういえば cron は 「クーロン」 って読むんじゃないの？ って聞かれたんですが、僕は 「クロン」 と読む派です。
何が正しいの読み方なのでしょうか。
