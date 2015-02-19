---

title: "UITabBarControllerのMoreに表示される edit を消したいなー。"
date: 2013-01-09T22:30:00+09:00
comments: true
categories: ["iOS","UITabBarController"]
---

**「UITabBarControllerのMoreに表示される edit を消したいなー。」**と思いながら、なんてググればいいんだろーと思いつつも、ヘッダファイルをみていたら customizableViewControllers ってプロパティがあった。
迷わずに nil に設定した。うまくいった。

もし、UITabBarControllerを継承してるクラスがあるなら -viewDidLoad で処理してしまうのが早いです。

```
self.customizableViewControllers = nil;
```

ない場合、用意しましょう。といってもいいんですが、タブ内の UIViewController の viedDidLoadで

```
self.tabBarController.customizableViewControllers = nil;
```

でも、いけました。
**「継承してるコントローラあるのにわざわざ試したんだからねっ!」**

ちなみに、

    このプロパティは nil じゃない場合 "Edit" で表示されるコントローラをカスタマイズできてデフォルトはすべてのコントローラだよ。

的なことが書かれていました。なので空の配列を渡してもOKです。
