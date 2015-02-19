---

title: "ViewSourceMap が地味に役に立つ"
date: 2012-12-13T14:35:00+09:00
comments: true
tags: ["ruby","rails"]
---

[ViewSourceMap](http://r7kamura.hatenablog.com/entry/2012/12/04/141911)というのが地味に役に立ちそうなので導入してみた。

部分テンプレートを render して出力された前後にどの view をレンダーしたのかHTMLのコメントを挿入してくれる。

```html
<!-- BEGIN app/views/users/_form.html.haml -->
  <form />
<!-- END app/views/users/_form.html.haml -->
```

ついでに、partial以外のレイアウトとかメインのビューとかもついでに出力してみるのもありかなと思ったりもするけど、その辺は明確だし、時間ができたら fork してみよう

ソースコードも短いしRails の plugin 的なものを作ってみたいときにも参考になりそうでした。

[https://github.com/r7kamura/view_source_map](https://github.com/r7kamura/view_source_map)
