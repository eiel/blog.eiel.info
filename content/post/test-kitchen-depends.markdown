---

title: "gitで公開されてるクックブックに依存している時のmetadata"
date: 2014-12-24T12:32:00+09:00
comments: true
categories: ["chef"]
---

ふつうに書くしかない。

利用する際にはcookbooksディレクトリにあればよい。
test-kitchenをする際には、Berksfileにかいとけばよい。
以下のようになる。例はrbenv。

```
metadata
cookbook 'rbenv', git: 'git://github.com/fnichol/chef-rbenv.git', branch: 'v0.7.2'
```

このクックブックに依存したクックブックを書くとつらい目にあうような気がするので、READMEにしっかりかいておいたほうが良さそう。

どうしてこんな話が出てくるかというと、rbenvは系統が違うものがふたつある。

* [chef-rbenv](https://github.com/fnichol/chef-rbenv)
* [rbenv-cookbook](https://github.com/RiotGames/rbenv-cookbook)

chef-rbenvのほうは[supermacket](https://supermarket.chef.io/)にあるのだけどrbenv-cookbookのほうはない。お互いに関連はなさそう。

「[VagrantにRuby/Rails開発環境を整えるChef+Berkshelf構築メモ - Qiita](http://qiita.com/zaru/items/1436a383c3d41483c371)」ではgitで指定していたのでそんな話がでてきただけである。
