---

title: "認証必須環境におけるJenkinsのスクリプトトリガーによるビルドの実行"
date: 2012-11-19T18:18:00+09:00
comments: true
categories: ["jenkins"]
---

[Jenkins](http://jenkins-ci.org/)は便利に使わせていただいているのですが、git push をhookして`ビルドの開始`をできるようにしていないまま運用していて、重い腰をあげてやっと設定することにしました。

公開しているサーバやイントラにあるサーバであれば

`http://YOURHOST/jenkins/job/PROJECTNAME/build`

ヘ wget してしまえば良いので簡単です。インターネット上に置いているとそうもいかないので、認証を必須にします。

認証を必須にしている場合は

* ユーザID (USER)
* API Token (APITOKEN)
* Project Token (PROJECTTOKEN)

が必要になります。

API Tokenと Project の Tokenが別のものだと気がつかずに無駄にはまりました。

```
$ wget --auth-no-challenge --http-user=USER --http-password=APITOKEN 'http://jenkins.yourcompany.com/job/your_job/build?token=PROJECTTOKEN'
```

とすることで上手くいきました。

最終的には

```
$ wget -q --auth-no-challenge --http-user=USER --http-password=APITOKEN 'http://jenkins.yourcompany.com/job/your_job/build?token=PROJECTTOKEN' -O - > /dev/null
```
で回してます。

USER はユーザのIDをそのまま使えばよいです。
APITOKEN はユーザの設定画面にあります。
PROJECTTOKEN はプロジェクトの設定画面で自分で設定します。

# 参考文献

https://wiki.jenkins-ci.org/display/JENKINS/Authenticating+scripted+clients
