---

title: "Raphaelで遊んでみた。"
date: 2013-09-11T18:42:00+09:00
comments: true
tags: ["javascript","rahaeljs"]
---

[すごい広島 #17](http://great-h.github.io/events/event-17.html) でやったこと。
[Raphael.js](http://raphaeljs.com/) で遊んでみました。
Raphael.js は JavaScript で SVG を作成できるライブラリ。

今日作成した[サンプルコードはGitHub](https://github.com/eiel/raphael-sample)にup してます。stepごとに タグを作成しているので、STEP1 のコードがみたい時は `git checkout step-1` としてください。
`step-5` まであります。

`checkout` したタグで `rake server` を実行すると、ローカルサーバが起動します。
[http://localhost.com:8000](http://localhost.com:8000) にアクセスしてみてください。

### STEP1 とりあえず試す。

Raphael オブジェクトを作成すると自動的にSVGオブジェクトが挿入される。
絶対座標で挿入されるので、DOMの構築を待つ必要はなかった。

まずは円を描いてみる。

```javascript
var paper = Raphael(10, 50, 320, 200);

// 円を書く at x = 50, y = 40,  半径 10
var circle = paper.circle(50, 40, 10);
// 赤色でぬりつぶす
circle.attr("fill", "#e00");

// 黒で境界線をかく
circle.attr("stroke", "#000");
```

### STEP2 SVG を二つ作成してみる。

Rahael を二度呼べば構築できる。

```javascript
var paper = Raphael(10, 50, 320, 200);

// 円を書く at x = 50, y = 40,  半径 10
var circle = paper.circle(50, 40, 10);
// 赤色でぬりつぶす
circle.attr("fill", "#e00");

// 黒で境界線をかく
circle.attr("stroke", "#000");

paper = Raphael(10, 240, 320, 200);

circle = paper.circle(50, 40, 40);
circle.attr("fill", "#00e");
circle.attr("stroke", "#000");
```

### STEP3 アニメーションしてみる



2秒かけて移動させてみる

```javascript
var paper = Raphael(10, 50, 320, 200);

var circle = paper.circle(50, 40, 10);
circle.attr("fill", "#e00");
circle.attr("stroke", "#000");

// 2000ms かけて x座標 320 まで移動
circle.animate({'cx': 320}, 2000);
```

element に対し animate メソッドでアニメーションできた。


* [Element.animate](http://raphaeljs.com/reference.html#Element.animate)


第1引数にはアニメーション後の element の情報を指定する。

第2引数には アニメーションの実行時間を設定できる。
設定しないと 0 になり、一瞬で移動してしまう。

第1引数に指定できるパラメータは Element.attr を見ればよさそう。

* [Element.attr](http://raphaeljs.com/reference.html#Element.attr)

### STEP4 クリックイベントでアニメーションしてみる

円をクリックでアニメーションしてみる。

```javascript
var paper = Raphael(10, 50, 320, 200);

var circle = paper.circle(50, 40, 10);
circle.attr("fill", "#e00");
circle.attr("stroke", "#000");

circle.click(function () {
  circle.animate({'cx': 320}, 2000);
});
```

### STEP5 パスでも使ってみる。

好きな図形を書きたいので、パスをつかってみる。

```javascript
var paper = Raphael(10, 50, 320, 200);

// (10, 20) へ移動
// (30, 40) へ線をひく
// (10, 40) へ線をひく
// (10, 20) へ線をひく
var path = paper.path("M 10 20  L 30 40  L 10 40  L 10 20");
path.attr('stroke',"#000");
```

Paper.path を利用すると描けた。

* [Paper.path](http://raphaeljs.com/reference.html#Paper.path)

文字列で指定する。
[SVG path string Format](http://www.w3.org/TR/SVG/paths.html#PathData) というルールに従う。

### 感想

リファレンスだけでもプログラミングが充分できるほどインターフェースがシンプルでした。動的に走査できるので、ちょっと遊べそうです。とても簡単。
