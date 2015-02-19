---

title: "Exception Notification の 4.x 系へのアップグレード"
date: 2013-07-24T10:47:00+09:00
comments: true
tags: ["rails"]
---

Rails のプラグインである [Exception Notification](http://smartinez87.github.io/exception_notification/) を gem upgrade したらエラーが起きた。
例外が発生したらメールを送信してくれるやつです。
最近は [Errbit](http://errbit.github.io/errbit/) とかが流行りらしいです。
ブルジョワ層は [Airbrake](https://airbrake.io/pages/home) とか使うらしいです。

話が剃れた。起きたエラーは以下の通り。

```
undefined method `new' for ExceptionNotifier:Module
```

対処方法は公式に書いてある。

* [Upgrading to 4.x version - Exception Notification](http://smartinez87.github.io/exception_notification/#getting-started/upgrading-to-4-x-version)

* ミドルウェアの名前が `ExceptionNotification::Rack` に変更された
* 第2引数に `:email` という上位にキーが必要になった

production モードじゃないと発生しないので、Jenkins などで `rake assets:precompile` して置かないと発見が deploy するまで遅れそうです。
