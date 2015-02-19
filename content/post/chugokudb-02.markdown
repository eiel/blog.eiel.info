---

title: "第２回 中国地方DB勉強会に参加したり、喋ったり。"
date: 2013-10-05T18:00:00+09:00
comments: true
categories: ["database","ActiveRecord"]
---


[第２回 中国地方DB勉強会](http://local.aguuu.com/events/21550) に参加しました。

### O/R Mapping の話をするよ。特にActiveRecordの話をしたかった。

セッションしました。準備不足はいいわけにはできないけど、ぶっちゃけ準備不足で、対象者が詰め切れていませんでした。

とはいえ、スライドはアップロードしておきます。

<iframe src="http://www.slideshare.net/slideshow/embed_code/26883805" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="https://www.slideshare.net/TomohikoHimura/or-mapping-activerecord" title="O/R Mapping の話をするよ。ActiveRecord の話をしたかった。" target="_blank">O/R Mapping の話をするよ。ActiveRecord の話をしたかった。</a> </strong> from <strong><a href="http://www.slideshare.net/TomohikoHimura" target="_blank">Tomohiko Himura</a></strong> </div>

### よろしい、ならばMicro-ORMだ

ORM の利点を長所もわかったんだけど、 ORM が使えない場面で ORM がもつ機能の一部を使いたい。そんなときに Micro-ORM が便利だよ。という話がつづきました。

特にデータのマッピングを操作する部分の機能を持っているようでした。
たしかに、普段のプログラミング生活では表現力の高いクエリビルダと値のマッピングができれば充分な感じは最近していたりもします。

* [スライド](http://www.slideshare.net/kiyokura/microorm)

### MySQL Cluster 解説 ＆ MySQL Cluster 7.3 最新情報

MySQL と MySQL Cluster 異なる製品で、MySQLのストレージとして MySQL Clusterを利用したりできるようです。
単体でも使えるっぽいですが、よくわかっていません。
安価なマシンを並べて性能を出しつつ、高い可用性を実現します。

使われてる箇所としては、「艦コレ」で使われていたのが最近の話題っぽいです。

MySQLのストレージとして使えるので、InnoDBの互換性向上とかもやってるそうです。

* [スライド](http://www.slideshare.net/yoyamasaki/mysql-cluster-mysql-cluster-73)

### AWSで始めるPostgreSQL/MySQL

AWS で MySQL や PostgreSQL をいれる話でした。

[Wiki に内容がまとめられてます。](http://myhome.munetika.mydns.jp/ossdbwiki/index.php/%E5%88%9D%E5%BF%83%E8%80%85%E5%90%91%E3%81%91DB%E5%85%A5%E9%96%80)


### さくらインターネットにおけるデータベース提供の実際

特にレンタルサーバの話が印象的でした。

データベース不慣れな人が多く、そういった一部の人が共用サーバに不可をかけてしまったりするということは予想通り多いみたいです。
コントロールパネルが書籍との違いが出てしまうので、なかなかUIを変更できなっかたりもするそうです。

DBはIOがボトルネックになるので仮想化は向いてないのかなあ。なんて話もしてでていました。
