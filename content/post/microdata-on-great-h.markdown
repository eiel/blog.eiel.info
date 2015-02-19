---

title: "Microdata を使ってイベント情報をページに埋め込む。"
date: 2013-06-23T23:44:00+09:00
comments: true
tags: [microdata]
---

この記事は [すごい広島 #005](http://great-h.github.io/events/event-005.html) で調べたこととかです。その2。

[すごい広島](http://great-h.github.io) は個人的な思惑として、「広島にいる表に出てこないエンジニアを発掘する。」という裏の目的があったりなかったりします。

そのためには、SEOとして効果のありそうな技術もいろいろ試してみよう。

ということで、[Microformats](http://microformats.org/)を使ってウェブページにイベント情報を付与してみようと思いました。
以前、ブラウザの拡張を使えば、「カレンダーアプリケーションへイベント登録が簡単にできる。」という噂を聞いたことがあったからです。

調べてみると、HTML5 には Microdata というものがあるみたいなので、Microdataを試すことにしました。

しかし、結局カレンダーに簡単に登録する方法はわかっておりません。

# Microdata

[コーディングとSEOの概念が変わるかもしれない、Microdataについての概要 - Web Design KOJIKA17](http://kojika17.com/2011/06/summary-of-microdata.html) より引用

> Microdataとは何か？
>
> マークアップ言語であるHTMLは「見出し(h1,h2,h3... )」「段落(p)」「リスト(ul,ol,li)」などの文章構造を示すことができても、「人の名前」「肩書き」「地域」などを示すことができません。
> それらをHTMLでメタデータとして追加する方法のひとつとして、HTML5の仕様からMicrodataというものができました。

> 類似の技術には、MicroformatsやRDFaなどがあります。
> これらMicrodata,Microformats, RDFaを指す言葉として「Semantic HTML」などと呼ばれています。

一応自分の理解で説明してみます。

> 人間はページを読んでこれは「イベント情報だ」とか、「会社の情報だ」とか認識する能力がありますが、コンピュータにはわかりません。
> なので、コンピュータがわかるようにするためのルールを用意しました。
> このルールに従ってHTMLを書いてください。

というものだと思います。

仕様とかは下記をみてください。

* [Microdata Working Draft](http://www.w3.org/TR/microdata/)

# Microdata と Microformats

せっかく  Microfrmats の話もしたので、比較ページとしては下記のものが面白っかたです。

* [マイクロフォーマット(microformats) vs. マイクロデータ(microdata) - 檜山正幸のキマイラ飼育記](http://d.hatena.ne.jp/m-hiyama/20100412/1271032800)

# 実際に使ってみた

すごい広島のイベントページに使ってみました。
イベントの情報として、場所や日時、名前を設定しています。

[Google ウェブマスターツールの構造化データ テストツール](http://www.google.com/webmasters/tools/richsnippets?q=http%3A%2F%2Fgreat-h.github.io%2Fevents%2Fevent-004.html)を使うと内容や、Google検索での結果を確認することができます。
![構造化データテストツールの結果](/images/20130623-sample.png)

赤枠の部分に日付や場所が表示されています。

具体的な読取した結果も確認できます。

![サイトプロパティ](/images/20130623-result.png)

この部分のHTMLは下記のようにしました。

```html
<div itemscope itemtype="http://schema.org/Event">
  <h1>日時</h1>
  <span itemprop="startDate" content="2013-06-26T19:00">2013年06月26日 19:00</span>
  <h1>開催場所</h1>
  <div itemprop="location" itemscope itemtype="http://schema.org/Place">
    <span itemprop="name">タリーズコーヒー広島本通店</span>
    <div itemprop="address" itemscope itemtype="http://schema.org/PostalAddress">
      <span itemprop="addressLocality">広島市</span>
      <span itemprop="streetAddress">中区本通３−９</span>
      <meta itemprop="addressRegion" content="広島県">
      <meta itemprop="addressCountry" content="日本">
      <meta itemprop="postalCode" content="7300035">
    </div>
  </div>
  <meta itemprop="name" content="すごい広島 #6">
  <meta itemprop="image" content="https://raw.github.com/great-h/great-h_logo/master/logo.png">
</div>
```

実際には [Jekyll を使ったGithub Pages で関数呼び出し的なことをする](/blog/2013/06/19/jekyll-on-function/) で書いたテクニックを使っています。

[具体的にはこの辺に](https://github.com/great-h/great-h.github.io/blob/e264ccad9733ec534ef63f43b2aa551943173afc/_layouts/event.html)

# 書き方とか

書き方などは

* [Microdata + schema.org を実際に使ってみる - terkel.jp](http://terkel.jp/archives/2011/08/microdata-and-schema-org/) 
* [schema.org 日本語訳 - 始めましょう！](http://schema-ja.appspot.com/docs/gs.html)

などが参考になりました。

埋め込みする情報の構造は [schema.org](http://schema.org/) に集められているようです。
このあたりは LDAP を仕様したことあるとわかりやすいように思いました。
