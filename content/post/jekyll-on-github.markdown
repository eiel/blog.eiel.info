---

title: "Github で Jekyll を使う時に調べたこと"
date: 2013-02-18T00:46:00+09:00
comments: true
categories: ["github", "jekyll"]
---

Github で Jekyll を使うときにできることとか調べたので整理しておきます。
今日の成果物。
この記事をいきなりポーンと書いても仕方ない気がして前の記事を書きました。

* [Github Pages について整理しておきます](http://blog.eiel.info/blog/2013/02/17/github-pages/)

Jekyll を利用するかしないかの判断材料などに利用してください。

利用できる マークアップ

* HTML
* Markdown
* Textile

関連する gem

* [liquid](http://liquidmarkup.org/)- テンプレートエンジン
* [redcarpet](https://github.com/vmg/redcarpet) - 高機能高速動作な Markdown
* [maruku](https://github.com/bhollis/maruku)    - 高機能な Markdown
* [rdiscount](https://github.com/rtomayko/rdiscount) - 高速動作な Markdown
* [RedCloth](http://redcloth.org/)  - Textile
* [Pygemnts.rb](http://pygments.org/) - シンタックスハイライト

できること

* Layoutの利用 - ネスト可能
* includeの利用 - Jekyll Boostrap がかなり利用してる様子
* 記事の作成
  * atom.xmlや記事一覧の作成 - site.posts 変数から参照可能
* シンタックスハイライト

できなかったこと

* Rubyのコードを書いて改造
* pluginの利用
  * 対応するファイルのないものを自動生成
  * 拡張タグ
  * 利用できるタグの追加
* gem を読み込んで情報源などにする
* Scss, Sass, Less などメタCSSの利用
* CoffeeScript などの利用

Jekyll Bootstrap がしてること

* include を駆使して
  * デファルト値を設定したり
  * _config.yml で登録した値で分岐したり


## プログラムをかいてカスマイズはできそうにない

Github で Jekyll が動作する際に `--safe` オプションがつくため _plugins ディレクトリ内のファイルは実行されませんでした。
他にコードを読ませる手段が非合法な方法を探さないとできそうにないです。

知っていたら教えてください。

もし Jekyll を Rubyでカスタマイズして使って、Github Pages で公開したいのであればローカルでJekyll を動かして生成されたものを push しましょう。
github の push をフックして、別のサーバで動作させるのもありかもしれません。

なので、基本的には liquid redcarpet pygemnts を利用してページを作成していくことなります。

## Jekyll

Jekyll についての詳しいことは [30分のチュートリアルでJekyllを理解する](http://melborne.github.com/2012/05/13/first-step-of-jekyll/) という記事が非常に良いです。

## liquid

テンプレートエンジンです。
タグを使用する場合は `｛｛ 変数 ｝｝` を使い、変数を参照する場合は `｛% タグ %｝` を使います。(エスケープできないので全角を使用)

｛% 変数 %｝ でも 変数の参照ができますが、未定義の場合はエラーになります。

タグには ループをする `for` や 分岐を行う `if` 、SSIにあるような 外部ファイルを読み込む `include`などが使えます。
変数の利用や フィルター という文字列を加工する機能もあります。

詳しいことは公式の[ドキュメント](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)を参照してください。

変数は Hash であれば ドットでアクセスできます。

カスタムタグなど作る機能は用意されれてますが、Github Pages では利用できません。

## redcarpet

マークダウンを使う場合、選択肢がありますが、これが一番よさそうです。
C言語のライブラリ sundown のラッパーで 高速に動作し、[PHP Extars](http://michelf.ca/projects/php-markdown/extra/) という Markdown の拡張文法に対応しており、`_config.yml` で、利用の可否を設定できます。

例: `_config.yml`

```yaml
markdown: redcarpet
redcarpet:
  extensions: [tables,autolink,strikethrough]
```

といった感じに設定できます。

設定できる拡張は わかりにくいですが、 [README](https://github.com/vmg/redcarpet) の extensiots に書かれています。

* no_intra_emphasis
* tables
* fenced_code_blocks
* autolink
* strikethrough
* lax_spacing
* space_after_headers
* superscript

があります。

## pygemnts

シンタックスハイライトをするためのものです。

対応言語は[ここ](http://pygments.org/languages/)に書かれています。

```
  ```ruby
  def hoge
  end
  ```
```

のように使用します。

## Jekyll Bootstrap

Jekyll Bootstrap は「まずはじめたい。」ときにも良いですし、参考にしても役に経ちます。素のJekyll からはじめたい場合は参考になります。

## その他

動作確認をする場合は `_config.yml` に

```yaml
safe: true
lsi: false
pygments: true
```

と、書いておき、

```
$ jekyll --auto --server
```

として、ブラウザで確認するのがよいです。
`jekyll bootstrap`や`Octopress` には `rake preview` があります。

## まとめ

冒頭にまとめてます。

Jekyll とか liquid のソースコード読んでみたけど、カスタマイズの仕方もわかったけど、Github Pages 上で動かす分には無意味になった。

Jekyllをローカルで走らせるか、Scssなどはpushする前に自分で変換する。などの手法をとることになります。
この辺を自動化しているのが [Octopress](http://octopress.org/) になります。

蛇足ですが Jekyll の同様のツールで Haskellでカスタマイズする [Hakyll](http://jaspervdj.be/hakyll/) というものもあります。 
Jekyll はある程度ルールがありますが、 Hakyll はもっと自由度が高いツールになります。

## 関連

* [Git がわからなくても Github を利用しよう](http://blog.eiel.info/blog/2013/02/06/how-to-use-github/)
* [Github Pages について整理しておきます](http://blog.eiel.info/blog/2013/02/17/github-pages/)
