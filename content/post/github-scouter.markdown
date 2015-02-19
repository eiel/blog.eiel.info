---

title: "GitHub戦闘力を提案してみた - 座駆動LT大会"
date: 2014-09-13T22:38:00+09:00
comments: true
categories: ["ネタ","勉強会","GitHub"]
---

座駆動LT大会で「戦闘力」というLTをしてきました。

座駆動LT大会とは、岡山にはRyouteiという素晴しいお店があり、そこの座スタジアムという部屋は非常にLTに適した場所です。

> 大都会岡山が誇る最強の懇親会会場「Ryoutei 座・スタジアム」でLT大会を開催します！

というわけで、今回参加してきた時のスライドを紹介します。

<script async class="speakerdeck-embed" data-id="9f557cd01d560132ff4612198c64cd5d" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

戦闘力といえば、Vim戦闘力やEmacs戦闘力がありますが、GitHub戦闘力を適当に定義してみました。
スターの数がGitHub戦闘力と言われているのもみかけましたが、折角なのでいろいろ考えてみました。

実は[オープンソースカンフェレンス2014広島](http://www.ospn.jp/osc2014-hiroshima/)のために制作しているものの中でGitHub APIを使用してつくっていたものがあり、そのノウハウで、そのついでに作成したのが今回の`github_scouter`です。

* [eiel/github_scouter · GitHub](https://github.com/eiel/github_scouter)

オープンソースカンファレンスは今週末の土曜日、2014年9月20日に予定されています。
予約数があまり多くないらしいので、今後も継続して欲しいと考えている方は参加や告知を協力していただけると助かります。

ちなみに私は[LT駆動開発ベストセッションズ](https://www.ospn.jp/osc2014-hiroshima/modules/eguide/event.php?eid=7)でLTをする予定です。

閑話休題。
今回はGitHub戦闘力を攻撃力、知力、すばやさの3種類に分けて戦闘力を定義しました。

攻撃力は所有リポジトリを元に算出しました。

知力はさまざまな言語を利用していると高くなるようにしました。

すばやさはOrganizationの情報を元にチーム力の高さとして算出しました。

と、非常にどうでもいい戦闘力ですが、みなさんも御自分の戦闘力を算出してみてはいかがでしょうか。
また、APIの利用しすぎにご注意ください。

<blockquote class="twitter-tweet" lang="ja"><p>.<a href="https://twitter.com/eielh">@eielh</a> 「わたしの戦闘力は168Gです」 <a href="https://twitter.com/hashtag/zadrvnlt?src=hash">#zadrvnlt</a></p>&mdash; (っ’ヮ’c) ＜ 君のほうがかわいいよ (@ryosms) <a href="https://twitter.com/ryosms/status/510772214694572032">2014, 9月 13</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
かけ算バージョンは表計算で出したので、コマンドを用意していません。
0にならないように1を加えてからかけ算しています。

今後、計算式を定義しなおしてVersion2も検討したいと考えているのはまた別の話です。


# 追記

Gem化して欲しいって要望があったのでしておきました。

```
$ gem install github_scouter
$ github_scouter [GitHub ID]
```

で利用できます。
