---

title: "DockerでCMSの会 Vol.2に挑む - 準備編"
date: 2015-01-26T18:01:00+09:00
comments: true
categories: [docker]
---

[CMSの会 Vol.2](http://kawa-tani.com/cms-hands-on/)というイベントが2月7日(土)に広島で行われるらしい。
[a-blog CMS](http://www.a-blogcms.jp/),[concrete5](http://concrete5-japan.org/),[Wordpress](https://ja.wordpress.org/)の3つのCMSを使って、同一のウェブサイトを作成して、個々の長所短所を学べるらしい。

こっちも確認しておくとよいだろう。

* [川谷制作所 » CMSの会Vol.2をやります](http://kawa-tani.com/blog/?p=447)

さて、参加するかどうかは不明だが、参加条件に環境を準備をしてくる必要がある。
どうやって環境を用意するかと考えた結果。「**ここはDockerしかない**」と思い、環境を整えてみた。

実際に参加してみるとどんな問題にぶつかるかわからないので、参加する場合に参考にする場合は注意しておいたほうが良いでしょう。
事前にテキスト的なものがいただけたら確認はできるかもしれないが。

さて本題。

dockerがインストールされていれば以下のコマンドで環境を構築できる。
Windowsは想定していない。(そろそろOSが欲しいが空いてる64bitマシンもない)

```
docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD="secret" eiel/mysql:concrete5
docker run -d --name my-wordpress --link my-mysql:mysql  -p 8001:80 -v `pwd`/wordpress:/var/www/html eiel/wordpress
docker run -d --name my-concrete5 --link my-mysql:mysql -p 8002:80 -v `pwd`/concrete5:/var/www/html eiel/concrete5
docker run -d --name my-acms --link my-mysql:mysql  -p 8003:80 -v `pwd`/acms:/var/www/html eiel/acms
```

イメージをダウンロードするので時間がかかります。
二度目からはダウンロードの必要がないのでだいぶはやくなります。

* wordpress
* concrete5
* acms

の3つのディレクトリが作成されます。
それぞれのCMSの作業ディレクトリとなっていて必要なものが準備されています。

CMSの動作を見るにはdockerの動いているホストのそれぞれのポートにアクセスすればOKです。

boot2dockerを使っている場合はまずIPを確認します。

```
$ boot2docker config | grep LowerIP
LowerIP = "192.168.59.103"
```

IPが192.168.59.109だったと仮定すると

* Wordpress http://192.168.59.103:8001
* concrete5 http://192.168.59.103:8002
* a-blog cMS http://192.168.59.103:8003

でそれぞれ動作確認ができます。

![cms2](/images/2015-01-27/cms2.png)

動作を停止するには

```
$ docker stop my-wordpress my-concrete5 my-acms my-mysql
```

です。

再度、起動するには

```
$ docker start my-wordpress my-concrete5 my-acms my-mysql
```

となります。

concrete5とa-blog CMSはデータベースの指定をしないといけません。
以下の情報を使ってください。

* ホスト名: mysql
* ユーザ名: root
* パスワード: secret
* データベース: concrete5 (ablogの場合は acms)

を指定してください。

a-blog CMSのインストール後に`setup`ディレクトリの移動をしますが、`_setup`にしてください。
`_setup`以外のものにすると`docker run`をするときに`setup`ディレクトリを作ってしまってまた移動させる必要があります。

### やりなおす

Dockerのイメージを取得したところからやりなおしたくなったら、以下でなおせます。

```
docker stop  my-wordpress my-concrete5 my-acms my-mysql | xargs docker rm
```

もう一度起動する場合は代わりありません。

```
docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD="secret" eiel/mysql:concrete5
docker run -d --name my-wordpress --link my-mysql:mysql  -p 8001:80 -v `pwd`/wordpress:/var/www/html eiel/wordpress
docker run -d --name my-concrete5 --link my-mysql:mysql -p 8002:80 -v `pwd`/concrete5:/var/www/html eiel/concrete5
docker run -d --name my-acms --link my-mysql:mysql  -p 8003:80 -v `pwd`/acms:/var/www/html eiel/acms
```

確実に10秒以上待つことになるのでのんびりお待ちください。

### もっと詳しく

それぞれのDockerfileは下記にあります。

* https://github.com/eiel/docker-mysql
* https://github.com/eiel/docker-wordpress-ja
* https://github.com/eiel/docker-concrete5
* https://github.com/eiel/docker-acms

eiel/mysqlは5.6を利用した場合concrete5のインストールに失敗したので少しカスタマイズしてあります。
sql-modeが`STRICT_TRANS_TABLES`にならないように変更しています。

* 参考文献 http://www.ah-2.com/2014/05/13/mysql56_mysql_install_db.html

wordpressは公式のものを流用していますが、 日本語版をインストールするように変更しています。

* 参考文献 https://github.com/docker-library/wordpress

concrete5はWordpressのものを参考に作成しています。
-v をつけたときのパーミッションではまったのでapacheの起動ユーザを変更して対処しています。

基本的にはDocker公式のWordpressのDockerfileを参考にして作成しています。

eiel/wordpressは日本語版をインストールするように改造しています。
eiel/cnocrete5とeiel/acmsはそれを参考にちょろちょろいじっています。

仮想マシンで何度も実行するのは大変だけどDockerはだいぶ楽ですね。
そろそろ何か本番でつかっていきたいところです。
