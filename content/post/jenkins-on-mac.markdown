---

title: "Mac で jenkins"
date: 2013-07-23T11:45:00+09:00
comments: true
categories: ["jenkins"]
---

Mac で ローカルに「すごい cron」的に使うために Jenkins を入れて遊んでいます。
その辺の理由は [広島Ruby勉強会 #32](/blog/2013/07/08/hiroshimarb-32/) でも話しました。

特に動作に関連する点で気になった部分をメモしておきます。

* アップデートの方法
* 再起動する方法
* Jenkins を止める方法
* 日本語が化けるのを直す
* 127.0.0.1 でのみアクセス可能にする
* 実行ユーザの変更

インストールには Native Package を利用しました。
[http://jenkins-ci.org/](http://jenkins-ci.org/)

### アップデートの方法

`/Applacations/Jenkins/jenkins.war` を差し替えて、再起動すればOK。
最新版がある場合は「Jenkinsの管理」の画面にリンクがあるので簡単にダウンロードできます。

### Jenkins を再起動する

プラグインのインストールした時に再起動するを選択できるけど再起動しない。これは私の環境なのかもしれませんが、よくわかりません。

仕方ないのでシャットダウンの準備ができてる状態にして、プロセスを kill しています。
あとは `launchd` が自動的に起動してくれます。
「Activity Moniter」で java を検索して kill するのが一般的でしょうか。

`launchd` をunload して loadしてもよいです。
その場合は以下のようにします。

```
sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist
sudo launchctl   load /Library/LaunchDaemons/org.jenkins-ci.plist
```

### Jenkins を止める方法

Native Package でインストールすると launchd に登録され、起動時に自動的に起動するようになります。

これを止めるには以下のコマンドを使います。

```
sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist
```

アンインストールしようとして `/Applications/Jenkins` を捨ててしまうと、launchd が jenkins を起動しようとするため、エラーが黙々とログに溜まります。

### 日本語が化けるのを直す

出力にある日本語が化けてたので、 file.encoding=UTF-8 を設定しました。
`/Library/Application Support/Jenkins/jenkins-runner.sh` を編集します。
javaArgs の初期値に追加しました。

```
javaArgs="-Dfile.encoding=UTF-8"
```

本当はこのファイルを書き換えたくないけど、妙案が浮かばなかった。
アップデートする際に書き換えるファイルが `jenkins.war` なので気にしないことにした。

### 127.0.0.1 でのみアクセス可能にする

このままだと 同じネットワーク上にいる人がアクセスできてしまう。
セキュリティ設定しないと、Jenkinsの性質上、相手に好きなようにローカルPC を操作されてしまいます。

自分だけアクセスできるようにします。

```
sudo touch /Library/Preferences/org.jenkins-ci
sudo defaults write /Library/Preferences/org.jenkins-ci httpListenAddress -string 127.0.0.1
```

起動スクリプトが httpListenAddress を defaults で設定しておくと読むようになっています。他にも ポート変更などができます。

* [参考: Jenkins Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Thanks+for+using+OSX+Installer)

認証を付けるという手もあると思いますが、こちらのほうが安全だし、ログインの手間がありません。

### 実行ユーザの変更

cron の代わりに利用しているため、自分の権限で仕事してくれないと困る場面があります。
あまり良くはないですが、仕方なく変更しています。

`launchd` で実行ユーザを指定できるので、`/Library/LaunchDaemons/org.jenkins-ci.plist` を編集します。

```xml
<key>UserName</key>
<string>ユーザ名</string>
```

の部分を編集します。

あとは `JENKINS_HOME` に設定しているディレクトリの所有者が違うのであれば、変更しておきましょう。

「日本語が化けるのを直す」のところと同様にあまりいじりたくないですが、気にしないことにしました。

### まとめ的な

メモリやらCPUパワーがあまってないマシンではおすすめできませんが、それなりに便利です。
とりあえず試しておくのに、知っておきたくなりそうな情報を混じえながら個人的なメモをまとめてみました。
使い方がわかってきたらチームのワークフローに混ぜていきたいところですね。
