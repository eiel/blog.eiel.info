---

title: "Sensu を少しだけ触ってみた"
date: 2014-03-05T20:16:00+09:00
comments: true
tags: ["sensu","chef","ruby"]
---

ちょっと前に [Sensu](http://sensuapp.org/) を試した。
大したことは試してないのですが、日本語の情報もあまりないので試したことを記録しておこうと思う。

### Sensu って？

[Nagios](http://ja.wikipedia.org/wiki/Nagios) という統合監視ツールの置き換えを狙ったプロダクトのようで、Nagios のプラグインがそのまま使えます。
そもそも Nagios のプロトコルをそのまま使ってるようです。

同様のツールとして [Zabbix](http://ja.wikipedia.org/wiki/Zabbix) などありますが、結構毛色が違うツールだということを今回わかりました。(Zabbix は試したことがありますが、Nagios は試したことがないです)

Zabbix は全部入りみたいな感じで、これだけでなんでもできたりして、入門するには難しい感じです。

Nagios を利用する際にはグラフを書きたい場合は [munin](http://munin-monitoring.org/) などを併用する人も多いようです。
munin は個人的に設定が楽なので、ちょろっとした時に利用します。

そんなわけで、「いまどきの Nagios」 である Sensu を試してみようという流れです。

### まずインストールしてみる

どんなものかピンと来ない場合はまず動かしてみるほうがいいです。
Sensu をインストールするのに chef や puppet が使えるように公式から [sensu-chef](https://github.com/sensu/sensu-chef) や [chef-puppet](https://github.com/sensu/sensu-puppet) があり割と簡単にインストールできるようです。

手動でもそんなに難しいわけではなく [ドキュメント](http://sensuapp.org/docs/0.12/guide)を見ながらやればできると思います。

というわけで、今回は sensu-chef を試しました。

私が chef の初心者なので、その辺のメモも書いています。

[sensu-chef の README.md をみる](https://github.com/sensu/sensu-chef)とやり方が書いてあります。
[Vagrant](http://www.vagrantup.com/) をつかって動かすサンプルがあります。

```
$ git clone git@github.com:sensu/sensu-chef.git
$ cd sensu-chef/examples
$ gem install bundler
$ bundle install
$ librarian-chef install
$ vagrant up
```

とすると sensu-server, suns-clint, sensu-api, sensu-dashboard, Redis, RabbitMQ がインストールされます。

それぞれの関係は [ドキュメントに図示されています](http://sensuapp.org/docs/0.12/overview)

![](http://sensuapp.org/docs/0.12/img/sensu-diagram-4801b356.png)

sensu が収集している情報は sensu-api 経由、または sensu-dashboard にアクセスと取得できるようです。
この example を利用した場合、 [http://localhost:8080](http://localhost:8080) で sensu-dashboard アクセスできます。
Basic認証がかかっていて、デフォルトでは

* User名: admin
* password: secret

になってます。
ちなみに [attributes/default.rb](https://github.com/sensu/sensu-chef/blob/master/attributes/default.rb#L35-L36) に定義されています。
実際に運用する場合は、この値ををどこかで上書きすればよいです。
(chef はいろんなポイントで値を上書きできるっぽい)

### なんか監視してみる

監視項目の追加には check を追加するようです。

* [Sensu | An open source monitoring framework](http://sensuapp.org/docs/0.12/adding_a_check)

最初のサンプルは crond をチェックするもので、 `/etc/conf.d/check_cron.json` というJSONファイルを作成することになります。

この chef のレシピを使っていると chef の機能である Data Bag をつかってcheckの追加ができるようになっています。
`data_bags/sensu_checks` ディレクトリにJSONファイルをおくことで設定できるようになっています。
試しに `data_bags/sensu_checks/check_cron.json` を作成してみました。

```
{
    "id": "cron_check",
    "handlers": ["default"],
    "command": "check-procs.rb -p crond -C 1 ",
    "interval": 5,
    "subscribers": ["all"]
}
```

この JSON は `/etc/conf.d/check_cron.json` を生成するための[monitor](https://github.com/portertech/chef-monitor)というクックブックにより提供されている機能で読み込まれて使用されます。

このJSONを配置すると下記のような `/etc/conf.d/check_cron.json` が生成されます。


```
{
  "checks": {
    "cron_check": {
      "command": "check-procs.rb -p crond -C 1 ",
      "subscribers": [
        "all"
      ],
      "handlers": [
        "default"
      ],
      "interval": 5
    }
  }
}
```

似ているものですが違うものです。

Sensu は設定ファイルがJSONで書くことができて、Chefやなんかで設定を生成しやすいという特徴があることがわかりました。

あとは cron をとめたり、開始したりして遊んでみてみると良いと思います。

```
$ service cron stop   # crond 停止
$ service cron start  # crond 開始
```

crond を止めてみると下記のような状態になりました。

![](/images/2014-03-05-sensu.png)


### まとめ

まだまだわからないことが多いのですが、とりあえず Sensu を体験することができました。
RedisやRabbitMQ のインストールが必要ですが Chef を使えば特に設定はせずに体験することができました。

ついでに chef の Cookbook を読んで chefの勉強することもできました。

### 参考文献

* [portertech/chef-monitor · GitHub](https://github.com/portertech/chef-monitor)
* [Sensu | An open source monitoring framework](http://sensuapp.org/docs/0.12)
