---

title: "今さらながら turbolinksを試した。- 感想"
date: 2013-02-11T01:14:00+09:00
comments: true
tags: ["rails"]
---

今さらながら Rails4 の新機能の一つである [turbolinks](https://github.com/rails/turbolinks) を試してみました。

解説なんかは僕がするよりも様々な記事がもうあるので、あまり必要ないと思います。

* [Rails 4.0 に入る予定の turbolinks について調べた](http://willnet.in/40)
* [Rails4 Turbolinksのメモ #mtsmhack](http://d.hatena.ne.jp/deeeki/20121202/rails4_turbolinks)
* [Rails4でデフォルトで入るturbolinksがオープンリダイレクタと合わさると何でもできてかなり危険](http://blog.uu59.org/2012-11-19-turbolinks-breaks-cors.html)

どれも良い記事でした。

ということで、個人的感想とか

* production環境より development環境での効果が高い気がした
  * 体感ほんとに早い - development環境
* だいたいそのままで動く。動かなくなる Javascriptはやっぱりある。
  * fancyboxとか、でも、そういうのはそもそもRailsで扱いずらいものばかり。
* CSSやJavaScriptが切り変わるリンクには HTMLタグに data-no-turbolink属性をいれればよい。いれないと悲しいことになります。

## インストール

一緒に [jquery.turbolinks](https://github.com/kossnocorp/jquery.turbolinks/blob/master/README.md) も入れておきました。
これは簡単に言うと、"なにもしなかったら動かなくなってしまうもの"を減らすように工夫したものだと思います。
なんとなくで具体的に書くと、readyイベントでやってることをページロードするときにも実行するようにするんじゃないかと思います。(たぶん)

jquery.turbolinks は turbolinks に依存してないのでそれぞれGemfileに入れる必要がありました。

```
gem 'turbolinks`
gem 'jquery-turbolinks'
```

```
//= require turbolinks
//= require jquery.turbolinks
```

といった感じです。

## 感想をもうちょっと

###  体感ほんとに早い

Rails3 でも試せるので、体感しておくのは良いと思います。設定はとても簡単なので、どういうものかは知っておいたほうが今後の方針に役立ちます。
個人的には、規模が大きいプロジェクトでは develpoment 環境でだけでも使いたいです。

### 動かなくなったプラグイン

試したプロジェクトは fancybox という jQueryプラグインが使用されてる箇所がありましたが、ここが動かなくなりました。
そもそも、Railsではこれはすごく扱いにくくて置き換えてやろうしてる部分で放置してるうちに次々と使われてしまってる問題児です。
修正するにはちょうどよい機会。

## まとめ

Rails的じゃないところほど問題がでやすい機能という点ですごく面白いなー。と、思いました。
