---

title: "AngularJSで遊んだときのメモ"
date: 2012-07-26T15:58:00+09:00
comments: true
categories: [AngularJS,hwebsys]
---

[広島ウェブシステム開発勉強会](http://hwebsys.eshima.info/)で、[AngularJS](http://angularjs.org/)で遊んだのでそのメモ。

## AngularJSってなんぞ

Googleとコミュニティによって開発されているWebアプリケーション作成のためのフレームワークらしいです。Googleという名がでてくるように、保守性の高そうな設計がしてある印象を受けました。非常に使いやすかったです。
DOM操作が必要にならない工夫もしてある印象。

特徴としては、

* 規約をもちいた、ViewとModelの自動化と knockoutJSのようにモデルの変更を検知して自動的に画面が更新される
* mix-inのようにコントローラへ機能を追加でき、引数からオブジェクトをうけとることで、スコープの制限をしていること
* htmlに埋め込まれる式は Javascriptではなく、AngularJSのDSLでシェルのパイプのような流れるようなコードが記述できる
* コンポーネント化しやすい構造

なのかなぁ。印象ですが。スコープをうまく設計してあると思いました。


## Angular

日本語の意味は角度っぽいです。由来が全然検討がつきません。

## チュートリアルをひととおりやってみました。

[http://docs.angularjs.org/tutorial/](http://docs.angularjs.org/tutorial/)

github で公開されてるサンプルコードを追っていきます。
node.js は必須ではないです。Mac では pkg が用意されていて簡単にインストールできるのでいれてしまったほうが楽だと思います。

以下、 各章のメモです。
step ごとの diff をみながらすすめぬのがよさそうです。
見ている step を checkout して`git show` などなど。


### step0 bootstrap

```
$ git checkout -f step-0
```
して
```
$ scripts/web-server.js
```
でサーバが起動する
http://localhost:8000/app/index.html
にアクセスすると Nothing here yet! と表示される。


* ng-app 命令

```
<html ng-app>
```

Angular Application のルート要素を指定する

* 二重の波括弧内に式がかける

```
{{'hoge'}}
```

かける式は Angular Expression であって JavaScriptではないらしい
ng-appの指定したモジュールを DOMContentLoaded イベント時に自動で読込む

* 手動でよみたい場合は

```
<script>
angular.element(document).ready(function() {
   angular.bootstrap(document);
});
</script>
```

#### 他の人がはまったポイント

* Javaばっかりしてるせいで localhost:8000を localhost:8080 でアクセスしていた。
* script/web-server.jsを プロジェクトルートで起動せず、index.htmlにアクセスできなかった

### step1 static Template

HTMLが普通にかけるねってことだった

### step2 Angular Template

* 直接埋め込んでいたデータをcontroller.jsに移動。
* コントローラはなにもしないけど、変数の割り当てをする。
* ちゃんとコントローラごとにバインディングされてる(すごくいい!)
* jasmineのサンプルもあってすごくいい

### step3 FilterRepeat

* inputタグでng-model属性を指定すると変数に代入されとりだせる

### Two-way Data Binding

* orderByを使用するとモデルの並び変えができる
* プロパティ名を指定するだけ

### XHRs & Dependency Injection
* $httpを使ってhttpアクセスができる
  * JSONを取得する場合は $http.json というのがある
    http://docs.angularjs.org/api/ng.$http#jsonp
    callbackには JSON_CALLBACKを指定すること

* $が先頭につくオブジェクトは特殊なオブジェクトっぽい。Rootオブジェクトにあるオブジェクトを共有してる感じ
* それを渡すかどうかは別に宣言するみたいだけど、ある程度暗黙になってる？

### Templating Links & Images

* ng-srcを利用すると画像のURLから表示できる

### Routing & Multiple Views

* 複雑になってきたので分離しようという章
* 詳細ページも用意する作業をします。
* $routeProbiderにルーティングを設定
  * URLに対しコントローラとテンプレートの関係を定義する
   * ng-app属性にmodule名を指定することで読込むモジュールを指定できる

### More Templating

* $routeParamsにルーティングによって代入される値が保持される railsでいうと paramsになります。

### Filterの作成方法
* 自前のフィルターを作成する方法
* モジュールを作成して filter関数を使用して定義することになる

### Event Handlers
* クリックされたときの処理を追加
* コントローラにメソッド追加して ng-click属性で呼び出し。オリジナルのプロパティを使うことでラッピングできてる。

### REST and Custom Serviwe
* angular-resource.jsを読込むとRESTfulなAPIにアクセスできる
* $resourceを使って Resourceを定義していく。


## まとめ

仕組みのなんだかわかりやすく、そこが見えていれば、AngularJS使ってるコードはよみやすそうでした。
HTMLをデザイナーが読めるかどうかが要めになりそうです。
