---

title: "UIWebView の UserAgent を変更した時に気をつけたいたった1つのこと"
date: 2013-12-10T02:04:00+09:00
comments: true
tags: ["iOS"]
---

iOS SDK のUIWebView は UserAgent を変更することができます。

* [[XCODE] UIWebViewを用いる際にUserAgentを独自に設定する方法](http://www.yoheim.net/blog.php?q=20121001)

UserAgent を変更したときに気をつけておきたいことを書きたいと思います。

**UserAgent によって条件分岐する JavaScript ライブラリがあることを忘れないようにしましょう。**

3時間以上たたかって、Google検索しても、なんにも情報ないし、「もうわからんわー」って投げ出したくなったところ JavaScript のライブラリのソースコード読み始めて、ようやく気付きました。
なるべくもとの UserAgent を尊重するほうが生きやすい世の中かもしれません。
