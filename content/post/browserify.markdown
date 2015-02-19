---

title: "nodejsのモジュールをブラウザで使えるようにするbrowserifyでちょっと遊んだ"
date: 2014-10-30T11:13:00+09:00
comments: true
tags: ["nodejs","javascript","browserify"]
---

[Browserify](http://browserify.org/)で少し遊んだ。

npmにあるライブラリをクライアントサイドで使いたいなぁ、という時に便利な子がBrowserifyさんです。
HTML側に複数のscriptタグを書かなくてよくなり、`<script src="bundle.js"></script>`のみ記述しておけば良いので管理が楽です。
(当然`bundle.js`以外の名前にすることもできます)

requirejsの代わりに使うこともできるし、gulpやらを組み合わせてminifyなどもできるでしょう。

### とりあえず試すには

QUERY_STRINGをクライアントサイドで処理するのを試した。
ライブラリには[qs](https://www.npmjs.org/package/qs)を利用。

1. nodejsをインストール
1. npm install -g browserify
1. npm install qs
1. qsを使うコードをかく
1. browserfyを実行
1. 生成されたjsを使う

nodejsのインストールは省略。

browserifyコマンドを利用するために`npm intsall -g browseriy`する。

今回はqsを使うので`npm install qs` をする。

以下のコードは`main.js`に書いた。


```javascript
var qs = require('qs');

console.log(qs.parse('aaa=bbb&ccc=ddd'));
```

出力は以下のようになった。

```
{ aaa: 'bbb', ccc: 'ddd' }
```

これをブラウザ上でもうごくようにするためにbrowserifyを使う。

以下のコマンドでbundle.jsを作成する。

```
$ browserify main.js -o bundle.js
```

qsの中身を`bundle.js`の中に加えて`require`を使える状態にもなってるらしいけど確認してない。

`index.html`を作成してブラウザで開くとコンソールで開いてみると同じ出力がでている。

```html
<html>
  <head>
  </head>
  <body>
    <script src="bundle.js"></script>
  </body>
</html>
```

### もうちょっとちゃんとQUERY_STRINGを解析してみる

さっきのは固定値だったので、ちゃんとURLから取得する。
`location.search`の値を使った。

main.js を以下のようにして、HTML上にも表示した。

```javascript
var qs = require('qs');

var queryString = location.search || "";
queryString = queryString.substr(1, queryString.length);
var params = qs.parse(queryString);
var json_text = JSON.stringify(params);
document.getElementsByTagName("body")[0].innerText = json_text;
```

### gulpと連携して変更検知して自動生成する。

以下に書いてある。

* [gulp/fast-browserify-builds-with-watchify](https://github.com/gulpjs/gulp/blob/master/docs/recipes/fast-browserify-builds-with-watchify.md)

`bundel.js`に補助の情報としてファイルの場所が表示されていたが絶対パスでリリースには使いづらかったので、`watchify.args.fullPaths = false;`にしてみた。

`main.js`をsrcディレクトリ移動して、出力先をdistディレクトリに変更しています。

gulpfile.jsは以下のとおり。ほとんどそのまま。

```javascript
var gulp = require('gulp');
var gutil = require('gulp-util');
var source = require('vinyl-source-stream');
var watchify = require('watchify');
var browserify = require('browserify');

gulp.task('watch', function() {
  watchify.args.fullPaths = false;
  var bundler = watchify(browserify('./src/main.js', watchify.args));

  // Optionally, you can apply transforms
  // and other configuration options on the
  // bundler just as you would with browserify
  bundler.transform('brfs');

  bundler.on('update', rebundle);

  function rebundle() {
    return bundler.bundle()
      // log errors if they happen
      .on('error', gutil.log.bind(gutil, 'Browserify Error'))
      .pipe(source('bundle.js'))
      .pipe(gulp.dest('./dist'));
  }

  return rebundle();
});
```

[watchfy](https://github.com/substack/watchify)は`browserify`のwatchモードにするためのものらしい。

gulp で実行できるようにするには、以下のようなことをした。

```
$ npm install -g gulp
$ npm install --save-dev gulp gulp-util vinyl-source-stream watchify browserify brfs
```

brfsはまだよくわかってない。

あとはgulpを起動すればよい。

```
$ gulp watch
```

作成したものはGitHubに投げています。

* [eiel/browserify-sample · GitHub](https://github.com/eiel/browserify-sample)

こちらは`npm run watch`でうごくようにしてあります。
以下のコマンドを実行すると動作確認ができます。
```
$ npm install
$ npm run watch
$  # index.htmlをブラウザでひらく。
```

`package.json` の scripts を使うと`npm install -g gulp`なしでも動かせるらしいので試した。

```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "watch": "gulp watch"
```
