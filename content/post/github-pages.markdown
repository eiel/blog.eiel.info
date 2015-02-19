---

title: "Github Pages について整理しておきます"
date: 2013-02-17T22:39:00+09:00
comments: true
tags: ["github"]
---

Git の練習を兼ねて Github できることといえばひとつとして [Github Pages](http://pages.github.com/) があります。
ウェブサイトを Git で管理して、Github へ プッシュすれば公開できるというものです。

使い方などは [公式のヘルプ](https://help.github.com/tags/20/articles)に書かれていますが自分が Github Pages を使おうとした時に知りたかったことを整理しておきます。
細かいことについてはあまり書きません。

* Github Pages の特徴
* Github Pages の種類
  * ユーザぺージ または グループページ
  * プロジェクトページ
* Github Pages の構築方法
  * Jekyll
  * 静的ファイル
* 独自ドメインの利用

## Github Pages の特徴

* 公開リポジトリで作れば無料。容量制限もないと言ってよいです。
* CGI,PHPなどで動的ページは生成できません。
  * 代わりに Jekyll というアプリケーションを使い github にページを生成させることができる

## Github Pages の種類

Github Pages には 2種類あります。

* ユーザページ または グループページ
* プロジェクトページ

### ユーザページ グループページ

ユーザページ と グループページは同じ機能と言えるので同じものと考えてください。

ユーザページはアカウントにひとつだけ作れる Github Pages になります。
グルーページは Github には Organization という グループを作る機能があります。
正確には `Organization Pages` ですが、グループページと呼びます。グループページもひとつだけ作れます。

このユーザページは http://`アカウント名`.github.com というアドレスでアクセスできるウェブサイトを作ることができます
。私の場合 アカウント名が `eiel` なので http://eiel.github.com になります。(作ってません)

グループも同じで hiroshimarb というアカウントのグループがあるのので [http://hiroshimarb.github.com](http://hiroshimarb.github.com) となります。
こちらは作っているのでこちらを例にしていきます。

このページを作るには、 [hiroshimarb.github.com](https://github.com/hiroshimarb/hiroshimarb.github.com) という リポジトリを作ります。

このリポジトリの **master** ブランチがウェブサイトになります。

自動生成機能はなく、自分で構築することになります。


### プロジェクトページ

こちらは リポジトリ用のページ作成をする機能です。
リポジトリごとに作れます。

アドレスは `http://アカウント名.github.com/リポジトリ名/` となります。
ユーザページのサブディレクトリに構築されます。

Hiroshimarb の [Hiroshimarb-gem](https://github.com/hiroshimarb/hiroshimarb-gem) というリポジトリのページは [http://hiroshimarb.github.com/hiroshimarb-gem/](http://hiroshimarb.github.com/hiroshimarb-gem/)になります。

これは、`hiroshimarb-gem` リポジトリの **gh-pages** ブランチがウェブサイトになります。

ユーザページとは違う点として 自動生成する機能があり、内容をウェブ上で入力して、レイアウトを選択することで作成することもできます。
自分で構築することもできます。

自動生成するには、 リポジトリの設定画面にいくと、Options に Github Pages という項目があるので、`Automatic Page Generator `をクリックして指示どおりすすめていくと作ることができます。

この自動生成されたページは Jekyll が使用されています。

## Github Pages の構築方法

何度か出てきましたが、Github Pages を使って公開するウェブサイトを構築する方法は大きく分けて二通りあります。

* Jekyll を使う
* Jekyll を使わない

です。

### Jekyll を使う

[Jekyll](http://jekyllrb.com/) を使う場合は markdown 形式や textile 形式のファイルを書いて push すれば、Github が HTML へ変換してくれます。
レイアウトなどの機能も備えているので 重複を抑えつつページを作成することができます。

ユーザぺージで、Jekyllを使う場合、デザインしたり、RSSを配信したりするのは少し手間がかかります。
楽をしたい場合は、[Jekyll Bootstrap](http://jekyllbootstrap.com/)などを使うある程度設定された状態と Jekyll を利用できます。

一応、もう一度書いておきますが、ユーザページなら master ブランチ、プロジェクトページなら gh-pages ブランチ にpush した内容が使われます。

### Jekyll を使わない場合はさらに細分化できます。

Jekyll を使わない場合は、ユーザページの master ブランチ、プロジェクトページなら gh-pages ブランチ の内容がそのまま公開されます。

なので、HTMLを書いてコミットして push すれば 普通のウェブサーバの様に利用することができます。

動的にページを生成することはできないので、JavaScriptなどで工夫したり、ローカルマシンで Jekyll などを動かして生成したページをコミットして使うという方法もよくされています。

[Octopress](http://octopress.org/) というツールがよく使われていますが、これはローカルマシンで Jekyll を動かしページを生成して、自動的にコミット、プッシュします。
同様のツールはいくつかありますが、ここでは紹介しません。

この方法の良いところはやりたい放題できることでしょう。Githubで動く Jekyll はplugin が利用できないなど、制限があります。

しかし、 Git の練習はできません。

## 独自ドメインの使用

使用したいドメインを `CNAME` というファイルに書いておいて、 DNSを設定をして,Githubを参照するようにすれば独自ドメインが利用できます。
DNSの設定方法にについてはここには書きません。

## まとめ

* Github Pages を使ってみたいなら ユーザページを試してみましょう。
* プロジェクトのサイトを簡単に作れます。成熟してるプロジェクトは試してみましょう。
* CGI、PHPの動かない Webサーバとして使えます。
* Blog を作りたいなら Jekyll Bootstrap か Octopress を試してみましょう。
  * Git を使う練習を兼ねたいなら Jekyll Bootstrap のほうがおすすめです。
  * 複数人で変更する可能性がある場合も Jekyll がよいです。
  * Scss などメタCSSを使いたいなら Octopress のほうが良いです。

説明してない結論がありますが、気にしないでください。

## 関連

* [Git がわからなくても Github を利用しよう](http://blog.eiel.info/blog/2013/02/06/how-to-use-github/)
