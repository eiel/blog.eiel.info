---

title: "広島Ruby勉強会 #031 で 「Hakyllで遊んだ」のでざくっと紹介した。"
date: 2013-04-07T01:00:00+09:00
comments: true
categories: ["hakyll","Haskell","active_support"]
---

[広島Ruby勉強会 #031](http://hiroshimarb.github.io/blog/2013/04/06/hiroshimarb-31/) で かるくLT しました。

内容は [Hakyll](http://jaspervdj.be/hakyll/) についてです。

なのですが、Rubyのリファレンスからメソッドの紹介をしているのですが、今回は ActiveSupport で追加される メソッド。Array 編をしました。

[その資料はこちらに。](http://railsdoc.eiel.info/)

この資料をどこにどうやって置こうかな？と思っていたので、ついでにHakyllを試してみました。そこで学んだこととかを紹介しました。

<iframe src="http://www.slideshare.net/slideshow/embed_code/18303056" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen webkitallowfullscreen mozallowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="http://www.slideshare.net/TomohikoHimura/hakyll-18303056" title="Hakyllで遊んでみた。" target="_blank">Hakyllで遊んでみた。</a> </strong> from <strong><a href="http://www.slideshare.net/TomohikoHimura" target="_blank">Tomohiko Himura</a></strong> </div>

このサイトのソースコードは [Github](https://github.com/eiel/railsdoc.eiel.info) に丸投げしていたりします。

このスライドに書いてないことでは、コンパイルを毎回するのがめんどくさかったので、ghci から 引数付きで main 関数を実行する方法を調べました。

`System.Environment` に定義されてる `withArgs` を使えばできました。

* `withArgs :: [String] -> IO a -> IO a`

利用例:

```haskell
withArgs ["build"] main
```

第1引数にコマンド引数をリストで渡してしまえば、良いようです。
