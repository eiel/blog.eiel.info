---

title: "Funtoo から Gentoo に戻してみた"
date: 2013-07-10T19:29:00+09:00
comments: true
categories: ["gentoo","funtoo"]
---

この記事は [すごい広島 #008](http://great-h.github.io/events/event-008.html) に参加中に書きました。

この間、我が家の Funtoo を Gentoo に戻してみた。

戻した理由 OpenRC のバージョン違いで munin が新しいのにできなかったから。
Gentooからの乖離が大きくなってきてるみたいですね。 #知らんけど

折角なので、戻す際に新規インストールせずに、そのまま移行できないか試みてみた。バックアップとるのがめんどくさかったとかでは決してない。たぶん。

Gentoo に関しては勉強不足なので、ここに書いてあることは**鵜呑みにせず参考にするぐらい**にしてください。
そういうこともできるだろうということでやってみたかった。

## 手順を考える

最初どうすればいいのかなぁ。考えてみた結果。
以下のとおりになりました。

* portage tree を Gentoo のものに変える
* emerge -e @system を実行する
* meerge -e @world を実行する

結論からいうと概ね予定通りできましたが、それなりにはトラブルがありました。

### portage tree を Gentoo のものに変える

一番苦戦したところです。

Funtoo は git を利用して portage tree を更新するので、参照先の Gentoo の rsync のものするだけでは `emerge --sync` がエラーになってしまいました。具体的には `/etc/make.conf` の SYNC です。

[Gentoo の portage tree も Git を使う](https://github.com/funtoo/portage) という手もあるのですが、これ最近 コミットされてないように見えます。
どこかに更新されてるのがあるのかもしれませんが、よくわからなかったのでこの方法は諦めました。

ということで、既存の portage tree である `/usr/portage` を一旦 `mv` で退避しておいて、 [http://rsync1.jp.gentoo.org/snapshots/portage-latest.tar.bz2](http://rsync1.jp.gentoo.org/snapshots/portage-latest.tar.bz2) を取得して展開することにしました。

差し替えたのはよいのですが、 `/etc/portage/make.pryfole` の形式が Funtoo と Gentoo で違いました。

Gentoo はシンボリックリンクなのですが、Funtoo は複数の profile が選択できるので、ファイルになっていました。
というわけで、手作業でなんとかしました。

### emerge -e @system

これで Gentoo のパッケージを参照できるようになったので、基本的なパッケージを差し替えていきます。
「一発でいけばいいなぁ」、と思いなが適当に回したところ、何度か失敗しました。

収録されているファイルが違うパッケージのものがあったみたいで、コンフリクトが起きました。
コンフリクトした先のパッケージを先にインストールして、再開すると無事に インストールできました。

### emerge -e @world

ここまでくるとほとんど Gentoo です。たぶん。
`emereg -e @world` は特に問題なく実行できました。
Gentooにないパッケージは `/var/lib/portage/world` を適当にいじりました。
具体的には、いらない行を削除しただけです。

### あと雑多な作業

`/etc/make.conf` の `SYNC` と `PORTAGE_MIRROR` を mirrorselect を使って設定しておきました。

## まとめ

適当に挑戦してみましたが、うまくいきました。
もしかすると何か不具合があるかもしれませんが、あればまた報告したいと思います。

何かの参考になるかもしれないと思ったので書き残すことにしましたが間違えてることも多々あるような気がします。
試すときはしっかりバックアップをとってから失敗しても良いようにして挑戦しましょう。

そういえば カーネル を変更していない。
