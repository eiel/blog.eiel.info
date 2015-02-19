---

title: "Cucumber-js を試した。"
date: 2013-09-03T15:49:00+09:00
comments: true
tags: ["cucumber","javascript"]
---

[広島Webシステム開発勉強会](https://twitter.com/hwebsys) で [Cucumber-js](https://github.com/cucumber/cucumber-js) を試してました。

先に雑感をかきます。

まだ完成度が高くない感じです。

* アサーションが用意されてないので、使いやすくするには自分でなんとかしないといけないような感じでした。
* step_definision が Ruby版より難しそうでした。
* 日本語への対応がまだできていませんでした。
* 色がまだつきません
* コマンドラインオプションをまちがえるとコールスタックが表示されます。

### インストール

```
npm install -g cucumber
```

`cucumber.js` というコマンドがインストールされます。

### 試してみる

```cucumber
Feature: hogehoge

  Scenario: hogehoge
    Given hoge
    Then goro
```

試しにこんな feature を書いてみました。

他に作成したのは `features/step_defnition/myStepDefinitions.js` と `features/support/world.js` です。

world.js で step で使える DSL を強化できますが今回は特になにもしていません。

Stepの定義は以下のように書きました。

```
var myStepDefinitionsWrapper = function () {
  this.World = require("../support/world.js").World;

  this.Given(/^hoge$/, function(callback) {
  // express the regexp above with the code you wish you had
     callback();
  });
  this.Then(/^goro$/, function(callback) {
  // express the regexp above with the code you wish you had
     callback.fail("gorogoro");
     callback();
  });
};

module.exports = myStepDefinitionsWrapper;
```

Ruby版に比べるとthisが目立ちやや不恰好です。
`Then goro` はわざと失敗するようにしています。

`cucumber.js` を実行すると下記のような出力になります。

```
.F

(::) failed steps (::)

gorogoro

Failing scenarios:
/Users/eiel/Programming/cucumber-js/features/myFeature.feature:3 # Scenario: hogehoge

1 scenario (1 failed)
2 steps (1 failed, 1 passed)
```

callback が重要で 成功、失敗、ペンディングのどれにするとしても利用します。
if で状態をチェック。問題があれば `callback.fail()` 問題がなければ`callback()` という流れになります。rsapec の `should` のようなものが用意されてません。

* [作成したサンプルリポジトリはGithubにupしてきました](https://github.com/eiel/cucumber-js-Sample)

### まとめ

というわけでなかなか扱いづらそうでした。
今度は mocha と chai あたりを試してみようと思います。

