---

title: "Polymer という Web Componets のラッパーを試した"
date: 2013-05-31T19:33:00+09:00
comments: true
tags: ["javascript","html5","polymer"]
---

Google I/O で紹介されていた [Polymer](http://www.polymer-project.org/) という JavaScript で少し遊びました。

Web Components という Web の UI を コンポーネント化するための仕組みがあります。
これをラップして 使いやすくしてくれる Polymer です。

[遊んだ結果はこの辺](https://github.com/eiel/polymer_experiment)

`rake preview` でウェブサーバが起動するので `http://localhost:4000` でアクセスできるようにしています。

ポイントは

* カスタムタグが作れる
* 属性でコンポーネントに情報を渡せる
* コンポーネントは独立した HTML ファイル
* linkタグ で読み込む コンポーネントを指定できる
* Web Components に比べて記述良が少ない

といっところでしょうか。


index.html は下記のようになっています。

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="polymer.min.js"></script>
    <link rel="import" href="my-element.html">
  </head>
  <body>
    <my-element hoge="goro"></my-element>
    <my-element hoge="mogu"></my-element>
  </body>
</html>

```

* Polymer を読み込んで
* my-element という コンポーネントを読み込んで
* my-element タグ で利用している

という流れです。

* hoge属性で my-element コンポーネントのボタンのラベルを変更しています。

my-element.html は下記のようになっています。

```html
<element name="my-element" attributes="hoge">
  <template>
    <div>
      <span>I'm <b>tk-element</b>. This is my Shadow DOM.</span>
      <button on-click="delete">{{ hoge }}</button>
    </div>
  </template>
  <script>
    Polymer.register(this,
    {
    "delete": function () { alert(this.hoge) },
    }
    );
  </script>
</element>
```

いくつか特殊なタグがあります。

element, template です。

* element の name属性に 作成するタグの名前を書く
* element の attributes属性に 外部から受け取る情報を登録する
  * hoge という属性でやりとりすることを書いています
* template の中にHTMLをかきます。
  * 波括弧を使うことで 変数の中身を取り出せます
* ロジック は Polymer.register を利用して登録します

非常にモジュラリティが高いので、HTMLが見やすくなりそうです。
