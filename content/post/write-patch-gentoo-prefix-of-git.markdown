---

title: "Gentoo Prefix 環境で git がビルドできないので bug報告してみた"
date: 2013-01-14T23:26:00+09:00
comments: true
tags: ["gentoo prefix"]
---

結構前からわかっていたんだけども、[Gentoo Prefix](http://www.gentoo.org/proj/en/gentoo-alt/prefix/) on Mac OSX で USE="subversion" していると gitのビルドに失敗していた。
なので、ビルドできるようにして、パッチを書いて [Gentoo Bugzila](https://bugs.gentoo.org/) へバグ報告してみた。

Gentoo Prefix というのは Gentoo Linuxのパッケージ管理である portage を /以外のところにインストールしていろんな環境で利用できるようにしているものです。役割的には MacPorts や Homebrew と同じように Mac で Unixツールをインストールするのに利用しています。


## どんなエラーがでていたか

USE="subversion" emerge gitすると
```
    LINK svn-fe
Undefined symbols for architecture x86_64:
  "_libintl_ngettext", referenced from:
      _show_date_relative in libgit.a(date.o)
  "_libintl_gettext", referenced from:
      _show_date_relative in libgit.a(date.o)
      _warn_on_inaccessible in libgit.a(wrapper.o)
      _xgetpwuid_self in libgit.a(wrapper.o)
ld: symbol(s) not found for architecture x86_64
```
svn-fe をビルドに失敗していました。

いろいろ調べてみると OSX 上では -lintl をつければビルドできることがわかりました。git をビルドするための Makefile はかなり凝ったものが使われてるのですが、その判定が svn-fe の Makefile にないため -lintl が自動でついていませんでした。

## どんなパッチをかいたか

CHOST で darwin がある場合 contrib/svn-fe/Makefile をかきかえるようなその場しのぎでかいてみました。

```
diff --git a/dev-vcs/git/git-1.8.1.ebuild b/dev-vcs/git/git-1.8.1.ebuild
index 1bfa55a..3338847 100644
--- a/dev-vcs/git/git-1.8.1.ebuild
+++ b/dev-vcs/git/git-1.8.1.ebuild
@@ -241,6 +241,12 @@ src_prepare() {
                -e '/$(INSTALL)/s/ $(libexecdir)/ $(DESTDIR)$(libexecdir)/g' \
                -e '/$(INSTALL)/s/ $(man1dir)/ $(DESTDIR)$(man1dir)/g'  \
                contrib/subtree/Makefile
+
+       if [[ $CHOST == *-darwin* ]]; then
+               sed -i \
+               -e 's:EXTLIBS =:EXTLIBS = -lintl:' \
+               contrib/svn-fe/Makefile
+       fi
 }
 
 git_emake() {
```

いろいろみていると `sed -i` で改行を入れてから sed の命令をかいていくスタイルが多いのでそれに従いました。

## どこに投稿したか

[https://bugs.gentoo.org/show_bug.cgi?id=452044#c0](https://bugs.gentoo.org/show_bug.cgi?id=452044#c0) に登録されています。
登録方法は

* アカウントを作成
* new をクリック
* Gentoo/Alt をクリック
* 類似バグがないか検索
* component は Prefix Support を選択
* Opereting System は OS X を選択
* summaryとdescription を記述
* 登録
* パッチを追加

という感じでした。

component には Mac OSX という項目がありますが、`DEAD. Do not use. See bug #214926.`の表示され、*使用するな!*ということみたいなので Prefix Support を選択しました。

## 他に学んだこと

### emereg *file名* で emerge するには

manifest が再計算されている必要があるようです。

ebuild があるディレクトリで

* repoman digest

または

* ebuild *file名* digest

で再計算されます。

また、ebuild ファイルは PORTDIR_OVERLAY を指定したディレクトリにいれておかないとダメみたいです。

### emergeの特定ステップのみ実行する

書いたpatch は prepare というコンパイルをはじめる前の段階に処理を追加しています。
なので パッチの動作確認だけであれば、そこまでで十分です。

* ebuild *file名* prepare

とすれば、そこまで処理することができます。
また、prepareが成功しているともう一度実行することができないので、

* ebuild *file名* clean

をした上で実行しないと反映されませんでした。
また、 ebuild を書き換えると

* ebuild *file名* digest

の実行が必要になります。これらをまとめて実行するには

* ebuild *file名* digest clean prepare

とすればいいことがわかりました。
