---

title: "LT駆動開発で「GitHubのリポジトリで先月のコミット一覧を見たい」という話をした"
date: 2015-01-10T18:27:00+09:00
comments: true
categories: [ltdd]
---

[LT駆動開発10](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA10)で最近つくったGemを紹介した。

<script async class="speakerdeck-embed" data-id="50b6d3907ad7013276ef56408e7781f2" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

作業しているGitリポジトリの先月の作業内容を閲覧するためにつくった。
GitHubのcompareのページをブラウザで開いてくれるGemだ。

```
$ git month-commits open
```

で使える。

openをけずると git log を実行します。

インストールは下記の通りである。

```
gem install git_month_commits
```


毎月やる作業を楽にするどうでも良いスクリプトである。

コミットを探す簡単なスクリプトを用意してたのだけど、折角なのでrubyで書き直した。

エコシステムが整備されてこういうのが気軽にインターネットにおけるようになったのは利点なのが問題点なのかわからないけど、楽しいので良いことにしよう。

ちょっと改造すれば先週とか月指定とかにもできるけど要望があったら考えることにしよう。

# 関連

* [eiel/git_month_commits · GitHub](https://github.com/eiel/git_month_commits)
* [LT駆動開発10でTest KitchenではじめるChef入門という話をした。OSH2015のステマかもしれない。](http://blog.eiel.info/blog/2015/01/10/chef-abc-on-test-kitchen/)
