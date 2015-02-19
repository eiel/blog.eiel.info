---

title: "Dart で遊んだ"
date: 2013-11-27T20:32:00+09:00
comments: true
tags: ["dart"]
---

[Dart](https://www.dartlang.org) の 1.0 がリリースされたらしい。
ということで、2013年11月の[広島Webシステム開発](http://twitter.com/hwebsys)で遊びました。

基本的には [Try Dart](https://www.dartlang.org/codelabs/darrrt/) を写経しました。

ウェブサイトから「Dart Editor」や「dart VMを組み込んだ chromium」や「JavaScriptに変換する dart2js」などなど一式含んだファイルをダウンロードできます。
Dart Editor を使えばすぐに Dart をはじめることができます。
ウェブアプリケーションも作成できますし、コンソールアプリケーションも作成できます。
コンソールアプリケーションは `dart` という実行ファイルが添付されているのでここから実行できます。

また、添付されている chromium も Dart VM を組み込みした特別なものみたいなので、Dartがいろんなブラウザで動くようになるのもまだまだ先になりそうです。
JavaScript に変換できるので、Dartで作成したアプリケーション自体は動かすことができるようですが、試していません。

あとは [Try Dart](https://www.dartlang.org/codelabs/darrrt/) を試していて気づいたこと書いていきたいと思います。

### セミコロンは必須

セミコロンかくのめんどくさい。

### Dart Editor

括弧とか「閉じ括弧」が補完されるようなものは入力が終わったときに TAB を押すと良い感じになるのを知った。
たぶん、eclipse はだいたいこのような挙動なのだろう。


### 添付chromium でも bootstrap に JavaScript が必要

Dartでつくったウェブアプリケーションは HTML に packages/browser/dart.js という JavaScript を読み込んでいて JavaScript から Dart エントリポイントが呼び出されるようです。「なんだって!」って気分でした。


### 他の Google 技術との親和性

例えば [Polymer](http://www.polymer-project.org) がすでに [Polymer.dart](https://www.dartlang.org/polymer-dart/) としてポーティングされてたりする。
Polymer はチュートリアルにも登場する勢い。
[Angular](https://github.com/angular/angular.dart.tutorial/wiki)もすでに利用できるみたい。

### `..` 演算子

`..` という演算子がいる。

```
genButton..disabled = false
         ..text = 'Aye! Gimme a name!';
```

`.` とほとんど同じなのだけど、`..` を使うとメソッドの戻り値を無視して `genButton` を返す感じになるようです。

```
genButton.disabled = false;
genButton.text = 'Aye! Gimme a name!';
genButton
```

と書いているのとほとんど同じ状態のようです。

代入するメソッドは void を返すのでメソッドチェーンできませんが、`..` があればメソッドチェーンできるようです。


### import show

import する時に名前空間をちらかさないようにできるみたいです。

具体例

```
import 'dart:math' show Random;
```

dart:math から Random というクラスかなにか知らないけど、そこだけ取り出せるようです。
Haskell にも同様の機能がありましたね。

### ファクトリメソッドが書きやすいらしい。

写経した中に、

```
PirateName.fromJSON(String jsonString) {
  Map storedName = JSON.decode(jsonString);
  _firstName = storedName['f'];
  _appellation = storedName['a'];
}
```

という部分があってクラスメソッドのように見えるんだけど、これから作成するインスタンスのフィールドにアクセスできてる。
別のサンプルで factory というキーワードをみかけたので、そういった機能なのだと思いつつ眺めていた。

### ローカルストレージがあつかいやすい？

言われるがままに写経してたけども、ローカルストレージを使うサンプルがあっさりと実装できた。JavaScriptでかくとどうなるのか調べて比較したいけど、そんな余裕はなかった。


### まとめ

まだまだ実際には使いづらいですが、外堀は着々と進んでいる印象を持ちました。


### 関連

* [Polymer という Web Componets のラッパーを試した](/blog/2013/05/31/polymer/)
* [AngularJSで遊んだときのメモ](/blog/2012/07/26/angularjs-abc/)
