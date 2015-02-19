---

title: "今後のiOSアプリケーションのために Auto Layout を学んだ - 内容編"
date: 2013-01-13T01:17:00+09:00
comments: true
tags: ["iOS","AutoLayout"]
---

[今後のiOSアプリケーションのために Auto Layout を学んだ - 準備編](blog/2013/01/13/ios-autolayout/) につづき勉強した内容をまとめていきたいと思います。

まずは Auto Layoutについて。概ね WWDC 2012 の session 202 のまとめだったり、使ってみた感じでのまとめです。仮説もいっぱい混ざってるので注意してください。

## Auto Layout は制約ベースのレイアウトシステム

Auto Layout は ふたつのViewの関係を設定していくことでレイアウトを構築します。
例えば *ある特定のViewは親のView左から 自分の左端を 20pt あける* のような制約をつくります。これらの制約を追加していくことで期待するレイアウトを構築します。

制約が無い場合はそれぞれデフォルトの振舞いがあります。
動きをみつつ上書きしていくような形になります。

制約のでViewのサイズなのが自動的に決定していくのでコードで記述する場合は frame の設定が不要になる書き方ができます。

## Auto Resizing Maskの機能を再現することもできる

Auto Layout は 以前のレイアウトシステム(?)である Auto Resizing Mask の表現範囲より大きなもので、エミュレートすることができます。Auto Resizing Mask でできることはすべてできますし、コードから利用する場合は今までどおりの挙動をします。

また、デフォルトでは Auto Resizing Mask をエミュレートしています。エミュレートさせたくない場合は translatesAutoresizingMaskIntoConstraints プロパティを NO に設定します。

## 作成できる制約

制約はふたつのviewに対して `item1.attribute1 = multiplier x item2.attribute2 + constant` という式を満たすように attribute1 を設定するようです。(たぶん) 等号の部分は不等号を指定することができます。制約はNSLayoutConstraint クラスの `constraintWithItem:attribute: relatedBy: toItem: attribute: multiplier: constant:`メソッドで作成することができます。 VisualFormatという言語を使用するともう少し簡単に制約を生成することもできます。

### Visual Format Language

この言語は アスキーアート的な書き方で制約を生成することができます。

例えば

```
|-20-[theView(200)]
```

theView という名前のviewを
- 親Viewの左端から 20 右に離す
- theViewの width を 200
という制約を作成することができます。

Visual Foramt を利用して制約を作成するには
NSLayoutConstraintクラスの `constraintsWithVisualFormat:options:metrics:views` メソッドを使用します。 metrics: の引数には Visual Format内で使用したい変数の NSDictionaryを。viewsにはVisual Foram内で使用したいViewの NSDictinaryを渡します。

先の例の 20 の部分を padding に変更したものであれば

```
NSString* format = @"|-pading-[theView(200)]";

NSNumber* padding = @20;
NSDictionary* metrics = NSDictionaryOfVariableBindings(padding);

UIView* theView = [[UIView alloc] init];
NSDictionary* views = NSDictionaryOfVariableBindings(theView);

NSArray* constraints = [NSLayoutConstraint constraintsWithVisualFormat:format
                                                               options:0
                                                               metrics:metrics
                                                               views:views]];
[self.view addConstraints:constraints];
```
のような風になります。

Visual Formatの表現力には限界があるので設定できない制約もあります。

## 満たせない制約があった場合

レイアウトする際に登録された制約する際に、満たせないものがでてくるとデバッグ出力として制約の一覧が出力され、非常にすばやく問題があることを検知できます。

## InterfaceBuilder での Auto Layout

InterfaceBuilderを利用して画面を作成していくと 磁石のように吸いつく場所におくと強制的に制約が生成されます。基本的にはその上に新な制約を付与していく形になります。この制約はInterfaceBuilder上で設定できますが、Outletにしてコード上で参照することもできます。

なので、InterfaceBuilder上でもかなりのことができますが、すべてではありません。

注意点としては
* Viewを移動すると制約が消えることがあります。というか、よく消えます。
* pin で制約を追加すると思わぬ制約がついていて削除していくことになる場合があります。
* Viewを移動したりサイズを変更したりが制約の影響を受けて変更できない場合があります。この場合は基準を変えたりするなど配置の仕方を工夫する必要があります。

## 制約の優先順位

制約には priority が設定できますが、初期は高い値が設定されています。
評価する順序に影響があるのではないかと予測しています。
不等号などを含む制約を後回しにすると 制約がコンフリクトしない場合がでてくるので下げるだけで済むようになっているんじゃないかなぁ。と、ふと思いましたが定かではありません。

## 勉強会中に作成したコードの一部

VisualFormatを試すのに作成したプロジェクトを[github上に](https://github.com/eiel/AutoLayout-Visual-Format-Language-Sample)置いているので興味がある方はどうぞ。

## まとめ

最初はわけがわかりませんでしたが、使用してみると思っていたほど難しくありません。うまく使えば Universal アプリも作れなくもありません。大きく表現を変えたい場合は別々に画面を作成したほうがよいです。プログラムは書いたとおりに動いてくれますが、制約も書いたとおりにレイアウトしてくれることでしょう。

図とか書きたかったですが、WWDCの動画などを参照してみてください。InterfaceBuilder上での作業はとても参考になります。
