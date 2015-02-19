---

title: "Rails で AngularJs を使おうとしてみた"
date: 2013-06-12T20:05:00+09:00
comments: true
categories: [angularjs,すごい広島]
---

[\<htmlday\> 2013](http://www.htmlday.jp)でやったことプラスアルファ。
足りなかった部分は[すごい広島 #4](http://great-h.github.io/events/event-004.html)内で調査しました。

\<htmlday\> では [AngularJS](http://angularjs.org/) で遊びました

* [前にAngularJSを利用したときの復習](http://blog.eiel.info/blog/2012/07/26/angularjs-abc/)
* [AngularJS](http://angularjs.org/) を rails で使うのに使えそうな gemの調査
* \<html ng-app\> と書けないの時の対処方法

### AngularJS を rails で使うのに使えそうな gemの調査


[とりあえず、Ruby Toolboxで angularを検索してみました。](https://www.ruby-toolbox.com/search?utf8=%E2%9C%93&q=angular)

候補としては

* [angular-rails](https://github.com/ludicast/angular-rails/tree/master/vendor/assets/javascripts)
* [anglurajs-rails](https://github.com/hiravgandhi/angularjs-rails/tree/master/lib)
* [angular-gem](https://github.com/ets-berkeley-edu/angular-gem)
* [angular-rails-engine](https://github.com/yjchen/angular-rails-engine)

と、乱立状態でした。他にもありましたけど、見る元気はありませんでした。

結論をまとめておくと

* 素の AngularJs でいいのなら angularjs-rails
* generatorなど、もしかすると便利になるものがあるかもしれないのが angular-gem

という感じでした。


まずは、angular-rails を試しました。generator などついていますが、AngularJSの本体が更新されていない状態でした。
また、添付されている angle.js を読み込みすると動作しなかったりと問題もちらほらありました。

次にみたのは angularjs-rails です。これは Javascriptが添付されているだけのシンプルなものでした。添付されているものも最新で、unstable な最新バージョンも添付されていました。読み込みたいだけならこれで良さそうです。x

unstableなバージョンをよみこむ場合は application.js に

```ruby
//= require_tree ./angular/unstable
```

と、かけば良さそうです。

3番目に angular-gem です。前述の angularjs をフォークして複数のversion のAngularJSを取りこんでいたりと、なかなか意欲的です。


4つ目は angular-rails-engin です。angular_include_tag というヘルパーを提供してくれて、必要に応じて読込みして使う感じになるようです。

とりあえず、 angular-gem を試していこうかなと思います。

### \<html ng-app\> を書けないのでその対処方法

AngularJs では

```html
<html ng-app>
```

のような書き方をして、機能拡張をする場合は、myModule を作成して、

```html
<html ng-app="myModule">
```

のようにします。

しかし、[haml](http://haml.info/) [slim](http://slim-lang.com/) を利用していると、自動生成されてしまって書けそうにない。

書けない場合は JavaScriptでなんとかできました。

```javascript
angular.element(document).ready(function() {
  angular.bootstrap(document, ['myModule']);
});
```

と書くと、前述のものと等価になります。
