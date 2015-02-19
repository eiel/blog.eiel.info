---

title: "cucumber で シナリオを Ruby のバージョンによって実行しなかったり"
date: 2013-04-11T01:48:00+09:00
comments: true
categories: ["cucumber"]
---

どんなにがんばっても ruby 1.8.7 では動かすことができない `cucumber` のシナリオがあって、これを対応した時のメモ。

結論から書いておきます。

* `@fails_on_1_8_7` というタグを使って、失敗するシナリオをタグづけ
* cucumber の profile に `ruby_1_8_7` をつくり `--tags ~@failds_on_1_8_7` を設定
* rake 実行時に RUBY_VERSION をみて profile を ruby_1_8_7 になるように設定

です。

テストは Travis CI で実行します。Travis では rake が実行されるので、rake 実行時にprofile を切り替えるようにしました。
もともとそのプロジェクトには ruby_1_9 というのと ruby_2_0 というのがあったのでそれらに合わせました。

具体的には [cucumber/cucumber に出したpull request](https://github.com/eiel/cucumber/commit/edfc124dd28abd92d41833bfdfd9018b754b4667) です。

## ちょっと掘り下げておく。

cucumber には profile という機能があって、あらかじめ設定しておいたオプションを切り替える機能があります。
`cucumber --profile ruby_1_8_7` みたいに使います。

これを rake task に割り当てるには `Cucumber::Rake::Task` のインスタンスのprofile に渡してやればいいです。
`new` したときのブロックの第1引数が `Cucumber::Rake::Task` そのものです。


例:

```ruby
Cucumber::Rake::Task.new do |t|
  t.profile = 'ruby_1_8_7'
end
```

cucumber のプロジェクトではは、この設定は [gem_tasks/cucumber.rake](https://github.com/cucumber/cucumber/blob/v1.2.5/gem_tasks/cucumber.rake)にあります。

ちなみに `Cucumber::Rake::Task` は [lib/cucumber/rake/task.rb](https://github.com/cucumber/cucumber/blob/v1.2.5/lib/cucumber/rake/task.rb)  に定義されています。

あとは travis が勝手に実行にしてくれる寸法です。

Cucumber のテストを Cucumber で書いてて紛らわしい例ですいません。

ちなみに、このpull request が取り込まれると以下のブログ記事にある修正が不必要になります。steps で日本語が使えます。

* [CUCUMBERで日本語でSTEPからSTEPを呼び出す - 自転車で通勤しましょ♪ブログ](http://319ring.net/blog/archives/2145)

## 関連記事

* [Cucumber のフィーチャの文法 - Gherkin](http://blog.eiel.info/blog/2013/02/12/gherkin/)
