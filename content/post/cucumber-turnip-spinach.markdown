---

title: "CucumberとTurnipとSpinachと。"
date: 2014-05-01T01:37:00+09:00
comments: true
categories: ["ruby","cucumber"]
---

最近 spinach というライブラリがあることを知って Cucumber や Turnip と同じようなものだということはわかっていたのですが、ちゃんと調べてみることにした。

* [Cucumber](http://cukes.info/)
* [Turnip](https://github.com/jnicklas/turnip)
* [Spinach](https://github.com/codegram/spinach)

「きゅうり」と「かぶ」と「ほうれん草」ですね。
一応ざっくり解説しておくと [ビヘイビア駆動開発](http://ja.wikipedia.org/wiki/%E3%83%93%E3%83%98%E3%82%A4%E3%83%93%E3%82%A2%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA) を実践するためのテスティングフレームワークです。
[Gherkin](https://github.com/cucumber/gherkin) という書式を利用して自然言語をならべて記述した文書を使い、自動テストとの結びつけができます。
動くことを確認することができる仕様書として使えます。

今回登場している3つのソフトウェアは Gherkin を使っている Cucumber がら派生したライブラリです。
Turnip は Cucumber から派生して、使いやすく改良したものです。
Spinach は Cucumber から機能を削減して見通しをよくしています。

### Turnip

Cucumber から派生して、Cucumber のイケてないところが修正されており、最近徐々に人気が出ているようです。
[るびま](http://magazine.rubyist.net/?0042-FromCucumberToTurnip)で取り上げられているので知っている方も多いと思います。
また rspec コマンドから実行することになります。


### Spinach

Spinach は [GitLab](https://github.com/gitlabhq/gitlabhq/blob/master/features/steps/help.rb) で利用されています。
Cucumber から強い機能が外されてます。
ステップから引数をうけとったり、シナリオテンプレートが廃止されていたり。
「重複を排除するための機能は Ruby のレイヤーでやらせてしまおう」という感じがしました

また、Gherkin は独自のものが再実装されていて国際化がされてないので、Cucumberだとできることが一部できません。
When や Given などは、日本語で「前提」や「もし」とかけましたが、Spinach 日本語が使えません。


### もうちょっと詳しく

例として Feature をひとつ作成してみました

```
Feature: Cucumber と Turnip と Spinach
ちょっと遊んでみる

Scenario: 配列の作成
  Given 長さが10の配列を作成
  When "string"を最後尾に追加
  Then 配列の中身は"string"である
```

features ディレクトリに保存しています。

まずはCucumberを試してみます。

```
$ cucumber
```

と実行すると下記のような出力があります。

```
Given(/^長さが(\d+)の配列を作成$/) do |arg1|
  pending # express the regexp above with the code you wish you had
end

When(/^"(.*?)"を最後尾に追加$/) do |arg1|
  pending # express the regexp above with the code you wish you had
end

Then(/^配列の中身は"(.*?)"である$/) do |arg1|
  pending # express the regexp above with the code you wish you had
end
```

「ステップがないので追加しろ。雛形は用意した。」そんな感じですね。
数字やダブルグオーテーションで括った部分は arg1 として利用できるように生成されます。


次に turinp を試してみます。
rspec コマンドから実行できます。

```
$ rspec -r turnip/rspec features
```

出力結果には Cucumberのように雛形が出力されたりはしませんでした。
なれていないとここからの作業は少し難しいかもしれません。

```
Pending:
  Cucumber と Turnip と Spinach 配列の作成 長さが10の配列を作成 -> "string"を最
後尾に追加 -> 配列の中身は"string"である
    # No such step: '長さが10の配列を作成'
```

rspec の一部のように動くようになります。

spinach も使ってみます。

```
$ spinach
```

Cucumber との大きな違いは正規表現で特定の値を抽出がなくなっています。
Turnip ではプレースホルダになりますが、 Spinach で利用できなくなっています。
**使わない**という思想のようです。

クラス定義になっており、使えるメソッドを増やすにはミックスインを利用することになります。

```
Feature: Cucumber と Turnip と Spinach
    Could not find steps for `Cucumber と Turnip と Spinach` feature


    Please create the file cucumber_turnip_spinach.rb at features/steps, with:

    class Spinach::Features::CucumberTurnipSpinach < Spinach::FeatureSteps
      step '長さが10の配列を作成' do
        pending 'step not implemented'
      end

      step '"string"を最後尾に追加' do
        pending 'step not implemented'
      end

      step '配列の中身は"string"である' do
        pending 'step not implemented'
      end
    end
```

### まとめ

簡単な比較しかしていませんが、spinach 面白いと思います。
もうちょっと試して紹介したいと思います。
Cucumberのシナリオテンプレートは使ってみると少し闇な感じがしていたので、使わないという割り着もよいと思います。
テーブルなんかは Rspec を使うより優位性を感じますが、この場合使えません。

あとは、ふるまいを記述する人がふるまいに集中できる感じがします。

### 関連

* [Cucumber のフィーチャの文法 - Gherkin - そんなこと覚えてない](http://blog.eiel.info/blog/2013/02/12/gherkin/)
* [cucumber で PhantomJS を使う - そんなこと覚えてない](http://blog.eiel.info/blog/2013/05/23/cucumber-with-phantomjs/)
* [Cucumber-js を試した。 - そんなこと覚えてない](http://blog.eiel.info/blog/2013/09/03/cucumber-js/)
