---
date: 2017-03-06T00:28:36+09:00
title: "広島フロントエンド勉強会で、カスタムエレメントの話をして、divタグの代わりにはつかえるんじゃないか?というLTをした"
---

[広島フロントエンド勉強会 Vol.7](https://hfe.connpass.com/event/50535/)でライトニングトークをした。案の定早すぎると言われた。いつものことである。(結構削ったんだけどな)

<script async class="speakerdeck-embed" data-id="e671bec0e21b43848359549693500de4" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

「[すごい広島](http://great-h.github.io/)」で[A-FRAME](https://aframe.io/)触ってる人がいて、A-FRAMEをみていたらカスタムエレメントをつかってるみたいだったので、調べたついでに思ったことを述べた。

「カスタムエレメントは独自のタグが使える」ぐらいの理解で充分かなっとおもってます。
Angular2にしても、Reactにしても、Riotにしても、Vueにしても独自タグのようなものがでてくるので、独自タグを定義できるという感覚は慣れておいて損はないと思い話を考えました。

本題ですが、カスタムエレメントは要素が定義するまでは未解決の要素となり、正当なエレメントになるらしい。

> 要素は定義の通りにアップグレードされるまでの間、 未解決の要素 と呼ばれます。これらの HTML 要素は正当な Custom Element 名を持ちますが、まだ登録されていないものです。
> 
> https://www.html5rocks.com/ja/tutorials/webcomponents/customelements/

ということは、エレメントを登録しなくてもつかえそうだし、特に中身を実装しなくても、使うことで可読性の高いマークアップができるような気がしました。

というわけで、特に何も実装してないカスタムエレメントでマークアップをすることは、カスタムエレメントの入門としてありなのではないかと思ったわけです。

> div だらけでモダンと言えるでしょうか？そして現状では、これが我々のウェブアプリの作り方なのです。悲しいですね。 我々はウェブプラットフォームからの恩恵をもっと受けるべきだとは思いませんか？
> https://www.html5rocks.com/ja/tutorials/webcomponents/customelements/

カスタムエレメントの誕生の意義からも全く外れていません。

独自タグの要素にCSSも問題なく使えます。定義しておかなくてもバッチリスタイルもあたります。

カスタムエレメントのAPIはv0とv1があるようです。
v1のほうが対応ブラウザが多いですが、v0はPolyFillもありますし、class構文が必要ないので、v0 APIのほうが実質対応ブラウザが多い気がします。class構文無しでもv1 APIは使えるみたいですが、結局未対応ブラウザはAPIが足りなくてつかえなさそうでした。(調べたメモがどっかいったのでソースは省略)

あとカスタムエレメントに対して知っておきたいことは、

* 独自タグの名前にはハイフンを含む必要がある
* カスタムエレメントを定義する前に利用しても良い
* まだAPIが変わる可能性がある
 
かなとおもいました。


「独自タグの名前にはハイフンを含む必要がある」というルールをみてA-FRAMEはいい名前をとってるなと感じました。`a-`で始まるタグはA-FRAMEのタグになるので、完全に一等地ですね。
Z-FRAMEはどんなライブラリがとるのかとても見ものです。

カスタムエレメントをマスターしたら次のステップはShadow DOMなんですかなー。

### ためしにかいたコード


```
<my-app>
  <my-contact>
    <my-header>連絡帳</my-header>
    <my-user>hoge@example.com</my-user>
  </my-contact>
  <my-inbox>
    <my-header>受信箱</my-header>
    <my-mail>2016-01-02 hogehoge</my-mail>
    <my-mail>2016-01-03 hogehoge</my-mail>
  </my-inbox>
</my-app>
```

```
body {
  display: flex;
  justify-content: center;
  align-items: center;
}

my-app {
  display: block;
  background-color: #eee;
  width: 300px;
}

my-contact {
  display: block;
  margin: 10px 10px 0 10px;
  background-color: white;
}

my-contact > my-user {
  padding: 4px;
  font-size: 10px;
}

my-contact > my-header {
  font-size: 10px;
}

my-inbox {
  display: block;
  margin: 10px;
  background-color: white;
}

my-header {
  display: block;
  padding: 0.2 0.5em;
  background-color: #823;
  color: white;
}

my-mail {
  display: block;
  padding: 4px;
}
```

https://playcode.io/6719


# 参考文献

* https://webkit.org/blog/7027/introducing-custom-elements/
* PolyFill [WebReflection/document-register-element](https://github.com/WebReflection/document-register-element)
* [whatwg Custom Elements](https://html.spec.whatwg.org/multipage/scripting.html#custom-elements)
* [Custom Elements | MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Custom_Elements)
* [Custom Element | HTML5Rocks](https://www.html5rocks.com/ja/tutorials/webcomponents/customelements/)
