---

title: "CentOSにJenkinsを入れてみた"
date: 2012-09-04T22:25:00+09:00
comments: true
categories: [CentOS,Jenkins]
---

ちょっと前に(だいぶ前?)にCentOSに [Jenkins](http://jenkins-ci.org/) をインストールしてみた。その方法のメモ。

Jenkinsは継続的インテグレーションを行うためのツールで、
ビルドやテストの自動実行を行い、カバレッジなどの統計データを生成したりするツールです。大規模なプロジェクトになってくるとローカル環境ですべてのテストを実行するのが難しくなってきたりすると便利です。

具体的なインストール方法ですが

```
sudo yum install java-1.6.0-openjdk
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum install jenkins
sudo cp -p /etc/sysconfig/jenkins /etc/sysconfig/jenkins.orig
sudo /sbin/chkconfig jenkins on
sudo /etc/init.d/jenkins start
```

といった感じです。JREを何使えばよいのか迷いましたが openJDKにしてみました。
あとは デフォルトーでは ポート `8080` で動作するので、 http://localhost:8080 へアクセスしたりすればよいです。
`/etc/sysconfig/jenkins` をいじりたい場合(ポートを変えたいなど)に備えてコピーもしてあります。

Jenkinsに関連するデータは `/var/lib/jenkins/` に配置されます。
