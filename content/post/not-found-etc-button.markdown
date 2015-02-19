---

title: "ツイートを埋め込みするためのボタンが行方不明なので、JavaScriptでなんとかする"
date: 2014-06-02T10:28:00+09:00
comments: true
categories: ["twitter"]
---

ブログにツイートを埋め込みしたい…。
なぜか「その他」ボタンがみつからない…なぜだ…。

![その他ボタンどこ…](/images/2014-0602-etc-button.png)

解析したら要素は存在している模様。ブックマークレットをつくってごまかす。

```
javascript: $('.permalink-tweet .embed-link button')[0].click()
```

ちなみに、ログインしてない状態だと「その他」ボタンが出ることに後で気づいた。
