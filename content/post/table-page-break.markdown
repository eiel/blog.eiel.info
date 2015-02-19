---

title: "HTMLを印刷時 表の途中で改ページを防ぐ"
date: 2012-11-22T09:49:00+09:00
comments: true
tags: ["HTML","CSS"]
---

印刷用のページってどうやって作るのがいいんだろう? PDF? HTMLでもいいの？
ってことで HTML でかきはじめてました。

キャプションと表をひとくくりにしたかったのですが、そのままだと表の途中で改ページされてしまうのは具合が悪くてなんとしたい。
ひとつの表はページを跨いで欲しくない。

CSSで改ページを制御するには `page-break-inside` を `avoid` にしてやればよいらしいです。
なので以下のようなHTMLを用意して、

```html
<div class="block">
  <h1>表の説明</h1>
  <table>
    <tr>
      ...
    </tr>
  </table>
</div>
```

CSSを以下のように用意すると期待したとおりになりました。

```css
.block {
   page-break-inside: avoid;
}
```
