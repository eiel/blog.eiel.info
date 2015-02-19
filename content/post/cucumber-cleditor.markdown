---

title: "cleditor の内容を javascirptで変更する in Cucumber"
date: 2013-05-31T19:20:00+09:00
comments: true
categories: ['cleditor','cucumber']
---

Cucumber の @javascript で実行しているシナリオに ]cleditor](https://github.com/cleditor/cleditor/blob/master/jquery.cleditor.js) という WYGSYG が利用されていて 普通に値を代入しただけだと反映されない問題に直面した。

caybara  のドライバーには poltergeist を使用しています。

バリデーションをかけていて、入力しないと進めないので、javascriptを使って入力することにした。

```javascript
$(selector).data('cleditor').$area.html( content );
```

.data('cleditor').$area というところに情報が保存されていることがわかったので、そこのHTMLをさしかえます。
Safari で実行した場合は画面表示は変更されないので注意です。

これを cucumber の step で実行したいので、

```ruby
description = "hogehoge"
code = <<"JAVASCRIPT"
$('#hogehoge').data('cleditor').$area.html('#{description}');
JAVASCRIPT
evaluate_script(code)
```

として、 evalute_script を利用して実行しました。

data属性にデータを保存しておくのは一般的なのかな？この辺の事情はよくしりません。
