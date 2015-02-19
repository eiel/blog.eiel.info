---

title: "LT駆動開発09で「Vagrantを使ったa-blog CMSの環境で思ったことを話した"
date: 2014-12-08T15:26:00+09:00
comments: true
categories: ["ltdd","ablog"]
---

[LT駆動開発09](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA09)でLTをした。

ちょっと前に[a-blog CMS](http://www.a-blogcms.jp/)の作業環境を[Vagrant](https://www.vagrantup.com/)で作成したのでその時に思ったことを話しただけである。
おまけでVagrantについての解説をしたがきっと必要のない情報だろう。中身がない。

<script async class="speakerdeck-embed" data-id="4d02497060470132157b26e128a6f114" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

Vagrantは簡単に開発環境を作成するツールらしいです。
Vagrantfileがあれば環境が何度でも構築できるのがポイントだろう。
あと設定が楽チンである。

a-blog CMSをchefでインストールするには[ogom/cookbook-acms](https://github.com/ogom/cookbook-acms)をいじったものを使用している。
いじったものは公開していなくてさーせん。

どんな問題があったかというとローカルで編集したファイルが全然反映されない問題がおきてつらかった。
あとは他の人がプロジェクトをクローンして`vagrant up`しても実行できない問題がおきた。
それについて説明しただけである。

というわけで、作業用のコードとCMSのコードは分離して配置できるといい気がするという話をしただけである。
a-blog CMS だと `config.server.php` でテーマディレクトリ位置は変えられるようなのでうまく使うと良さそうだと気づいが次に使う機会がない限りにそのままである。

中身がない。

ついでにVagrantのBOXは[Vagrant Cloud](https://vagrantcloud.com/)のものを使いたいけど、[bento](https://github.com/opscode/bento)で作成されているBOXを使いたいという話をした。
