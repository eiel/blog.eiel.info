---

title: "やっと Chef Solo はじめた"
date: 2013-11-13T18:28:00+09:00
comments: true
categories: ["chef"]
---

[すごい広島 #26](http://great-h.github.io/events/event-026.html) にて、書いている。

すこし前に chef-solo で遊んだので、その時思ったことを書いておく。
自分が考えたことが書いてあるだけなので、不正確な内容も含むかもしれません。

主な参考文献は以下。

* [入門 Chef Solo - Infrastructure as Code](https://www.amazon.co.jp/dp/B00BSPH158/ref=as_li_ss_til?tag=eiel-22&camp=1027&creative=7407&linkCode=as4&creativeASIN=B00BSPH158&adid=153X49YMKHPZN8FFFA70&)
* [今っぽい Vagrant + Chef Solo チュートリアル - Qiita - taiki45](http://qiita.com/taiki45/items/b46a2f32248720ec2bae)

「今っぽい Vagrant + Chef Solo チュートリアル」は「入門 Chef Solo」の内容を踏まえた上で、最近の動向も押えてて参考になりました。

### 目的

chef-solo を試す上で、
一番の目的は「リモートサーバの設定反映をコマンドを一つ実行すれば済むようにしたい」ということでした。

```
$ knife solo cook ホスト名
```

このコマンドをローカルマシンで実行すると、「ホスト名」が示すサーバの設定をできるようにしました。

また、「入門 Chef Solo」では、ローカルマシンでテストするにも `knife solo` を利用していた点が気になっていました。そこは別にしたかったので、`vagrant` の機能を利用して、

```
$ vagrant provision
```

で、済むようにしました。


### 成果物

[GitHub の eiel/cookbook-munin-example](https://github.com/eiel/cookbook-munin-example) に置いています。
使い方も README.md に書いています。

なぜ munin かというと、munin のインストールが必要だったからです。

上記のリポジトリは必要なツールがそろっていれば `git clone` して `vagrant up` すれば、`chef-solo` が走り、設定の終わった仮想マシンが立ち上がります。

Cookbook を修正して、再実行する際には `vagrant provision` とすれば反映できます。

### chef-solo を実行について

chef-solo の実行には、固有情報を記述した「JSONファイル」が必要になります。
「JSONファイル」はエントリーポイントのような位置づけで、実行するレシピの記述ができます。
Cookbook は汎用的なライブラリのようなもので、プログラミングに例えると「引数を与える必要があったり、呼び出すメソッドを指定する」必要があり、そういった指定するためのものが、「JSONファイル」になります。
設定ファイルとも言えます。

### Cookbook について

以下は Chef 使ってみた結果で構築された個人のイメージを記述します。

Chef を Ruby におきかえると Cookbook に相当するのが gem です。
Rubygems ではなく、ひとつの gem です。

Chef を試すときに最初に作成する `Cookbooks` (複数形のほう)  は gem を保存しておく場所と考えると良いと思いました。
Chef を使う場合は、トップレベルのスクリプトを書くところがなく、いきなりディレクトリ構造の整理した場所にコードを書かされます。

別の誰かが書いた Cookbook を使う場合は、「JSONファイル」の run_list にレシピを指定するだけで chef を利用できるわけです。
Cookbook はいろんなレシピをまとめていて、Chef を Ruby に置き換えると「レシピはクラスのようなもの」です。
gem 添付されているクラスを使うかどうかは、コードを書くときに決めることです。
chef だと run_list にかくことで使われます。

つまり Cookbook を書くというのは、いきなり gem を作るような感じになります。<br>
なるほど、これは難しい。


### knife solo について

`knife solo` は `chef solo` を外部サーバで実行するためのツールです。

`chef solo` を実行するには、設定したいサーバにログインして、利用する Cookbook をダウンロードして、jsonファイルを用意します。
これはめんどくさいので、リモートサーバを指定して、用意している Cookbook をアップロードして、リモートサーバで `chef solo` を実行します。


### なぜ knife solo でテストの実行をしたくないか

Vagrant にIPアドレスを設定する必要があり、「JSONファイル」をコミットしなければならなかったからです。
`IPアドレス.json` ファイルをgitリポジトリにコミットするのが嫌でした。

vagrant の box に chef がインストールしなければいけない問題は [vagrant-omnibus](https://github.com/schisamo/vagrant-omnibus) プラグインで解決しました。
このプラグインは `vagrant up` した際に、chef がインストールされていない場合にインストールしてくれます。


### provider

レシピで使える命令を追加できる Chef の機能です。
名前空間が Cookbook ごとに閉じているようで、利用したいクックブックの `metadata.rb` に記述しておかなければ、追加命令が使えません。
ちなみに、事前にそのクックブックのレシピを実行していれば、利用できました。はまる原因になりそうなので、`metadata.rb`には、ちゃんと書いたほうが良いと思います。


### vagrant ssh

`vagrant ssh` には `--` につづいて ssh の引数を追加できます。

vagrant にホストオンリーアクセスできるように指定してないので、とりあえず、これを利用してサーバのサービスにアクセスしてます。

```
$ vagrant ssh -- -L 4000:localhost:80
```

としておけば、 `https://localhost:4000/` で、Webサーバにアクセスできます。
若干めんどくさい…。

### vgrant provision

vgrant provision で使う「JSONファイル」は用意する必要がなく、Vagrantfile に同等の記述を行います。

以下の風に書いている。

```
config.vm.provision :chef_solo do |chef|
  chef.cookbooks_path = ["cookbooks", "site-cookbooks"]
  chef.add_recipe "munin"
end
```

knife-solo をベースしたリポジトリになっているので `site-cookbooks` を追加しています。
ひとつの cookbook を書くためのリボジトリであれば、不必要でしょう。

site-cookbooks にあるファイルを汎用化してきたら、個別のリポジトリをつくると良さそうです。

### その他

Databags や Attributes, librarian-chef も試したけど記憶からすでにない…。
serverspec はまだ試せてない。
