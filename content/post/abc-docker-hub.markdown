---

title: "Docker Hub を少し試してきた"
date: 2014-07-10T15:04:00+09:00
comments: true
categories: ["docker","gentoo"]
---

[Docker](http://www.docker.com/) 初心者です。
難しいことはよくわかりません。

いまさら [Docker Hub](https://hub.docker.com/) で遊んだ。

ちなみに [#すごい広島 60](http://great-h.github.io/events/event-060.html) で遊んでたことらしいです。


### 今回は何をしたのか

docker さえインストールされていれば、以下のコマンドを実行できます。

```
$ docker run eiel/banner
######   #######   #####   #    #  #######  ######
#     #  #     #  #     #  #   #   #        #     #
#     #  #     #  #        #  #    #        #     #
#     #  #     #  #        ###     #####    ######
#     #  #     #  #        #  #    #        #   #
#     #  #     #  #     #  #   #   #        #    #
######   #######   #####   #    #  #######  #     #
```


```
$ docker run -e BANNER_ENV="happy hacking" eiel/banner
#     #     #     ######   ######   #     #
#     #    # #    #     #  #     #   #   #
#     #   #   #   #     #  #     #    # #
#######  #     #  ######   ######      #
#     #  #######  #        #           #
#     #  #     #  #        #           #
#     #  #     #  #        #           #


#     #     #      #####   #    #  ###  #     #   #####
#     #    # #    #     #  #   #    #   ##    #  #     #
#     #   #   #   #        #  #     #   # #   #  #
#######  #     #  #        ###      #   #  #  #  #  ####
#     #  #######  #        #  #     #   #   # #  #     #
#     #  #     #  #     #  #   #    #   #    ##  #     #
#     #  #     #   #####   #    #  ###  #     #   #####
```

BANNER_ENV を設定すると出力を変えられます。

これの何が嬉しいかというと、**banner コマンドの実行環境が簡単に用意できる**ことだと思います。
redmine を実行するのに、 GitLab を実行するのに、Wordpressを実行するのに `docker run` でリポジトリ名を指定するだけでできるとしたらとても嬉しいと思います。

(実際にはDbのデータどうすんの？とかある気がするけど、どうするのがいいのかよく知らないけど)

定時実行するスクリプトが Docker さえインストールしていれば実行できたりするわけですね。

### Docke Hub はどこで使ってるのか

eiel/banner というイメージが Docker Hub にあいてあるので、`docker run eiel/banner` を実行すると、まだローカルにイメージがなければ取得して実行できます。
それだけ。

[このイメージはここにあります](https://registry.hub.docker.com/u/eiel/banner/)

[イメージを作成するのに Dockerfile をつかっており、コードは GitHub に up してあります。](https://github.com/eiel/docker-banner)

```
FROM eiel/gentoo-sample:banner
MAINTAINER Tomohiko Himura <eiel.hal@gmail.com>

ENV BANNER_ENV docker
CMD banner $BANNER_ENV
```

このファイルをカレントディレクトリに置いて `docker build -t banner .` とするとイメージを作ることができます。
`docker run banner` とすることで、同じことができるようになります。

Docker Hub は GitHub と連携して、`git push` した際に、`docker build` をして自動的にイメージをつくってくれる機能があります。
これで docker ファイルを編集して GitHub に push するたびに最新のイメージを自動的に作成され、Docke Hub に置かれるようになります。素敵。

Docker Hub でリポジトリをつくる際に automated build を選び、GitHub のリポジトリを指定するだけでした。

複数のリポジトリに紐付けしたり、途中から別のリポジトリに紐付けしたりはできませんでした。

`CMD banner $BANNER_ENV` と指定している部分が run に引数を指定しなかった時のコマンドになるそうです。

### もうちょっと詳しく リポジトリ連携

Dockerfile をみると `FROM eiel/gentoo-sample:banner` となっています。
元になるイメージが別にあります。

[このイメージはこっちにあります。](https://registry.hub.docker.com/u/eiel/gentoo-sample/)

banner はタグです。

[これも Dockerfile が GitHub にあります。](https://github.com/eiel/docker-sample-eiel-gentoo-banner/blob/master/Dockerfile])

```
FROM eiel/gentoo
MAINTAINER Tomohiko Himura <eiel.hal@gmail.com>

WORKDIR /usr
ADD install-portage.sh .
RUN sh /install-portage.sh
RUN emerge banner
CMD banner docker
```

emerge するために portage を追加して、emerge banner をしているだけです。

ADD するときに .tar.bz2 を展開できたら便利なのに…。

これも git push すると自動的にイメージが作成されます。
ところで、 eiel/gentoo-sample:banner が更新されたら、先ほどの eie/banner のイメージも構築しなおして欲しいですよね。

Docker Hub に Repository Links という機能があるらしいので、
eiel/banner のほうの Reposityr Links に eiel/gentoo-sample と追加すると eiel/gentoo-sample のイメージが新しくなると、eiel/banner も自動的にビルドされるようになりました。

### さらに詳しく eiel/gentoo はどうやってつくったか

FROM に eiel/gentoo と指定されてます。
これは Docker File を用意していません。

```
wget http://ftp.iij.ad.jp/pub/linux/gentoo/releases/amd64/current-iso/stage3-amd64-$20140619.tar.bz2 -O - | bzcat | docker import - eiel/gentoo
```

みたいな感じでつくりました。
こちらはイメージを作成したあと docker push eiel/gentoo にアップロードしました。

こちらも Repository Links を設定しておくとベースシステムの Gentoo が更新されると、eiel/gentoo-sample/banner が更新され、 eiel/banner が更新されるはずです。(試してないけど)

stage3 の tarball を展開しただけだけどこんなのでいいのかは [@naota さんのリポジトリをみてみたいと思う。](https://github.com/naota/dockergentoo)

個人的には portage をつっこんだイメージで emerge して 、portage をつっこんでないほうへもっていって使いたい。

### 蛇足

最初は sl コマンドをつかっていたのだけど run する時に `-t` 必要なのでやめた。

### まとめ

GitHub にプルリクをだすと実行環境として必要なイメージが作成されて、作成したイメージの上でテストコードを実行して、イメージのIDが返ってきて、その上でアプリケーションの実行とかできて、そのままデプロイできると素敵そうですね。失敗した場合は動作確認ができたりとか。

知らんけど

### 参考文献

* [Dockerfile - Docker Documentation](https://docs.docker.com/reference/builder/)
* [Command line - Docker Documentation](https://docs.docker.com/reference/commandline/cli/)
* [naota/dockergentoo · GitHub](https://github.com/naota/dockergentoo)
* [k2works/docker_practice · GitHub](https://github.com/k2works/docker_practice#ruby-on-rails%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AEdockerfile%E3%82%92%E3%81%A4%E3%81%8F%E3%82%8B)
