---

title: "Github の楽しみ方"
date: 2013-05-13T01:06:00+09:00
comments: true
categories: [github]
---

みなさん [Github](https://github.com/) を楽しんでいますか？

[まだ利用してない場合は、利用しましょう。](http://blog.eiel.info/blog/2013/02/06/how-to-use-github/)

「利用しはじめたけど、もう一歩進みたい…」という人のために、私なりの楽しみ方を紹介しておきたいと思います。

今回は以下の遊び方について書きます。

* 友人のリポジトリにちょっかいを出す
* 有名なリポジトリに名前を残す
* 毎日活動して Longest Streak の記録を更新する

### 友人のリポジトリにちょっかいを出す

Github は 「SNS」 です。
SNS なのでは人とコミュニケーションをとって遊びましょう。

なので、コミュニケーションをしましょう。
Github ではユーザ同士がコミュニケーションを取る主な方法は

* コミットへのコメント
* Issues
* Pull Request

があります。


こういったものはまずは友人に対して行うのが気軽で、オススメです。

しかし、リポジトリを作成したことや、コミットされたことに気づかなければ、コミュニケーションを取る機会がありません。

友人がリポジトリを作成したことに気がつくためには、友人をフォローしておくことです。
[以前紹介している]((http://blog.eiel.info/blog/2013/02/06/how-to-use-github/)ので、ここを読んでいる方はフォローしていると思います。

これで News Feed を見る癖がついていれば、友人がリポジトリを作成したことに気がつくようになると思います。
しかし、コミットされ、プッシュした情報は流れてきません。プッシュされたことを知りたいなら、`watch` をしましょう。

#### Watch する

`watch` をするには リポジトリのページへ移動し、以下の画像の示す部分を Watching にします。

![watch](/images/github-watch.png)
![watch](/images/github-watch-zoom.png)

`watch` するとそのリポジトリへの push や Issuesの作成、 Pull Request の作成などの情報が流れるようになります。
ここまで来れば日々チェックして、相手の隙を伺いアタックをしていきます。

#### コミットへのコメント

Github ではコミットに対してコメントをすることができます。
また、コミットに関連しているファイルの特定の行にもコメントを付けることができます。

まずは、Github で コミットを見てみましょう。

参考用に利用するリポジトリは [eiel/hiroshima_hall](https://github.com/eiel/hiroshima_hall) にしました。

コミットを見るにはコミットを見つける必要があります。
一般的な方法としては 「コミットの一覧」 > 「コミット」と進めばコミットのページへいけます。

コミットの一覧を見るには以下の画像を参考にしてください。

![commit list](/images/commit-list.png)

そこからコミットを見るには、見たいコミットの`コミットIDをクリックします。

![from commit list](/images/from-commit-list.png)

コミットに対しコメントしたい場合は画面下部にテキストエリアがあるので、そこに書き込みします。

また、ファイルの特定行数に対しコメントすることもできます。
コメントしたいところにカーソルを合わせると、左側に「+」のようなものがでますので、そこをクリックすると、フォームが表われます。

![comment](/images/github-comment.png)

さあ、コメントの仕方がわかったので、どんどんコミュニケーションをしましょう。マサカリは怖いです。

#### Issues

`Issues` は問題を発見した場合に、書き込むところですが、おおよそ掲示板のように使えます。

相手に伝えたいことがあるなら、書いてみましょう。

`Issue` を作るには、`リポジトリのトップ` > `Issues` > `New Issue` から行えます。

![github issues](/images/github-issues.png)
![github issues](/images/github-issue2.png)
![github issues](/images/github-issue3.png)

好きなようにメッセージを書いて遊びましょう。

各種コメントが書ける部分で #Issues 番号とすると関連付けもできます。
`:`(コロン) を書くと絵文字なども出せます。

#### Pull Request

Github といえば `Pull Request` です。
`Pull Request` は、「ちょっとこういう変更したら面白いじゃね？」とか、製作者に投げかける機能です。
ナイスな変更であればリポジトリの管理者の方が変更を取り込んでくれると思います。
そうでなくても、リポジトリに関した情報交換ができるでしょう。

`Pull Request` のやりかたは長くなりそうなので割愛しますが、簡単に書いておきます。
フォークをして自分のリポジトリを作り、ファイルの変更をすると pull request するボタンがでてきます。
これを押してメッセージを書くだけです。
ファイルを変更は Github 上でも行えまし、ローカルPCで編集して push する方法もあります。

![github pull](/images/github-pull.png)

`Pull Request` を繰り返してリポジトリへ貢献していくと、ちゃんと自分が貢献しことが記録されています。

![github graph](/images/github-graph.png)

[https://github.com/eiel/hiroshima_hall/contributors?from=2013-05-05&to=2013-05-12&type=c](https://github.com/eiel/hiroshima_hall/contributors?from=2013-05-05&to=2013-05-12&type=c)

大勢の人が参加しているリポジトリではないので、高い貢献率を叩きだせるかもしれません。
まるで、自分のリポジトリかのようにしてしまいしょう。


### 有名なリポジトリに名前を残す

友達のリポジトリを賑やかして遊んだら、本格的にリポジトリに絡んでいってみましょう。
自分が利用してるアプリケーションを `Watch` したり バグを直して `Pull Request`してみましょう。

僕は結構ショボイ Pull Request をしています。
例えば、[Ruby on Rails](https://github.com/rails/rails) で、一行削除しただけ `Pull Request` を出してみたりしましたが、取り込まれました。

* [rails #10339](https://github.com/rails/rails/pull/10339)

![rails pull1](/images/rails-pull1.png)
![rails pull2](/images/rails-pull2.png)

**これぐらいなら、できそうですよね!(えー)**

こうすることで、リポジトリに自分の名前が残りますし、
編集したファイルには小さいですが、自分のアイコンが出現します。

![rails pull3](/images/rails-pull3.png)

のらりくらりとソースコード読んで気になったところを Pull Request して自慢して遊びましょう。

### 毎日活動して Longest Streak を更新する

いつだったか忘れましたが、昨年末か今年の始めに Your Contributions という 自分の Github での活動が視覚化されるようになっています。

![Your Contribution](/images/github-contribution.png)

緑のマスが見える部分は一マスが一日に対応していて一年分表示されています。
濃い緑のところほど、貢献した日になります。
灰色の場合はなにもしてないことになります。

濃い緑がたくさん見えれば見えるほど「この人は化け物か!!」 なんて思ってしまうので、私もほどほどにがんばりたいところです。

この緑の部分が最高何日つづいたか、中央の Longest Streak に表示されます。
現在最高25日連続して貢献していることがわかります。

また、右隣の Current Streak は現在継続中の記録になります。
最高記録と並んでいる状態で、記録更新中の状態です。

これをがんばるために毎日更新できるようなものを考えてみたりしても良いかもしれません。
ちなみに、私はこれをするために [Ruby on Rails のソースコードを眺めてメモをとる作業をしています。](https://github.com/eiel/railsdoc.eiel.info/commits/master)

また、この緑の部分を利用して[音楽が聞けるサービス](http://song-of-github.herokuapp.com/?username=eiel)とかあったりして、Github の API を叩いて面白いサービスも作りたいですね。

### まとめ？

Github は日々進化していて、いろいろ面白い機能やら、便利になったりで目が離せません。

ITの世界に生きるのであれば、自己ブランディングするには現状では最強のサービスではないでしょうか。
自分なりの楽しみ方を見つけましょー。


### 関連記事

* [Git がわからなくても Github を利用しよう](/blog/2013/02/06/how-to-use-github/)
* [GitHub からの通知が迷惑メールになった - 見ないリポジトリは unwatch しよう](/blog/2013/11/21/github-notifications-is-not-spam/)
