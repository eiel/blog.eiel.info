---

title: "GitHub Pages で jekyll を使うなら safe: false で開発したほうが良いかもしれない"
date: 2014-05-08T09:50:00+09:00
comments: true
tags: ["jekyll","github"]
---

3月ぐらいから GitHub Pages でも使える Jekyll のプラグインが一部使えるようになりました。
最新の [github-pages gem](https://github.com/github/pages-gem) の v18 だと

* [jemoji](https://github.com/jekyll/jemoji) - GitHubの絵文字が使える
* [jekyll-mentions](https://github.com/jekyll/jekyll-mentions) - `@github-id` と、書くと自動でユーザへのリンクになる
* [jekyll-redirect-from](https://github.com/jekyll/jekyll-redirect-from) - 別のページからこのページに飛ばせる
* [jekyll-sitemap](https://github.com/jekyll/jekyll-sitemap) - `sitemap.xml` が自動生成される

が使えます。

ローカルで開発する場合、`_config.yml` に

```
safe: true
```

と記述してしまうと、これらのプラグインが動作しません。
GitHubに push すると動作してました。

ただし、GitHub上では `safe: true` の状態で動いてるはずなので、注意が必要です。(実際に確認はしてないけど)

github-pages v19 で Jekyll が 2.0.2 になるのでこれはこれでまた違ってくるかもしれませんが、確認していません。(まだリリースされてない)
そういえば、SASS とか CoffeeScript が使えるようになりそうなので非常に期待したい v19 です。

ローカルやTravisで生成すればだいたいのことができますが GitHub で生成できるとGitHub 入門として使いやすいですし、どんどん機能拡張されると良いですねー。

### 補足

プラグインを利用するには `_config.yml` に

```
gems:
  - jekyll-mentions
  - jekyll-redirect-from
  - jemoji
  - jekyll-sitemap
```

の記載が必要です。

当然不要なものがあれば削ちゃってください。

### 参考文献

* [Using Jekyll with Pages · GitHub Help](https://help.github.com/articles/using-jekyll-with-pages)

### 関連

* [Github Pages について整理しておきます - そんなこと覚えてない](http://blog.eiel.info/blog/2013/02/17/github-pages/)
[Github で Jekyll を使う時に調べたこと - そんなこと覚えてない](http://blog.eiel.info/blog/2013/02/18/jekyll-on-github/)
* [github-pages Gem というのが用意された - Github Page で使う gem のバージョンをあわせてくれる - そんなこと覚えてない](http://blog.eiel.info/blog/2013/08/13/github-pages-gem/)
* [すごい広島](http://great-h.github.io/)
