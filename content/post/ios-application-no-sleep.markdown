---

title: "iOSアプリでスリープしないようにしても、ホーム画面にもどったらスリープするよね"
date: 2014-07-28T18:19:00+09:00
comments: true
tags: ["ios"]
---

「iOS スリープしないように」で検索した結果、上位のいくつかの記事に

```
[UIApplication sharedApplication].idleTimerDisabled = YES;
```

とあり、これでうまく動作する。

注意事項として以下のこともかいてある。


> 上記の処理をするとアプリが終了してもスリープが起こらなくなってしまうため，アプリの終了時には必ず戻すようにします．
>
> [逆引きObjective-C for iPhoneアプリ - スリープモード（自動ロック）に移行しないようにする](http://www.objectivec-iphone.com/UIKit/UIApplication/idleTimerDisabled.html)

> ただし、このまま放っておくとアプリを終わらせてもスリープが起こらなくなるので、アプリの終了時には必ず戻すこと！
>
> [iPhoneSDKでスリープさせない方法 - 電子ガジェットいろいろ 開発メモ](http://d.hatena.ne.jp/uosoft/20100809/1281283605)

などなど書かれていて、事実なのか気になった。
UIApplication のインスタンスの設定を変えているのに OS の設定が変わるとは思えない。
というわけで、実際に試した。

普通にスリープしました。

記事自体も古いのでOSのバージョンによって違うのかもしれません。
