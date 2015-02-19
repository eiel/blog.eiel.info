---

title: "広島Ruby勉強会 #033 でやったこと - ActiveSupport CoreExt & Rails Template"
date: 2013-08-04T12:22:00+09:00
comments: true
categories: [rails]
---

[広島Ruby勉強会 #033](http://partake.in/events/9dacdbfc-8acf-4968-a0eb-5327a6937b7d) に参加しました。

前回に引き続き ActiveSupport のCoreExt のソースコードを読んだので、面白そうなところをざっくりと紹介しました。

* [資料 - Rails のソースコード読んだので面白そうなところを紹介する -- ActiveSupport CoreExt編 その2](http://railsdoc.eiel.info/hiroshimarb/33/)


あと LT しました。

`rails new` には `--template` という引数があるのでこれについて調べました。

まずはスライド。

<iframe src="http://www.slideshare.net/slideshow/embed_code/24887855" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen webkitallowfullscreen mozallowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="http://www.slideshare.net/TomohikoHimura/rails-template" title="Rails プロジェクトでスタートダッシュを決める" target="_blank">Rails プロジェクトでスタートダッシュを決める</a> </strong> from <strong><a href="http://www.slideshare.net/TomohikoHimura" target="_blank">Tomohiko Himura</a></strong> </div>

このスライドを作る前に調べものした時のメモは前回の記事を

* [Rails New した時の追加処理をかく](/blog/2013/08/03/rails-new-template/)

参加者の [もりしー こと @CentBoss](https://twitter.com/CentBoss) が 「[広島Ruby勉強会 #033に遊びに行ってrails newでのtemplateを作った](http://blog.mori-theta.net/?p=238)」 早速試してみてくれました。
`rails new` する時のデフォルトの引数を指定できる `~/.railsrc` というものがありますが、そのなかで --template が使えるのを試してくれています。
上手くいったようです。
ここでテンプレートファイルを指定しておくと、とても楽ができます。

次回の 広島Ruby勉強会は 9月7日を予定しているそうです。
発表の練習したい方募集中だそうです。
