---

title: "Mac でPostgreSQL"
date: 2012-10-04T22:20:00+09:00
comments: true
categories: [Mac,PostgreSQL]
---

railsアプリの一部を heroku にもっていこうとおもったので、「開発環境もPostgreSQLにすべきよね」と、思ったので 最初は Gentoo Prefix でインストールしたのですが起動方法がわからず。時間をかけたくなかったので、[One Click Installer](http://www.postgresql.org/download/macosx/)を使用したら Mac が起動できなくなりました。

というわけで、困っていたら[Postgres.app](http://postgresapp.com/)というのがありました。9.1.3ですが heroku が提供してるものなので、herokuにデプロイする目的には最適なような気がしております。

はい、それだけです。
