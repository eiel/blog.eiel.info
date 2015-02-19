---

title: "Rubyの 1.8 スタイルの Hash を 1.9 に書き換える"
date: 2012-12-30T14:30:00+09:00
comments: true
tags: ["ruby"]
---

[syntax_fix](https://github.com/HeeL/syntax_fix) を使うと一瞬でした。

```
(defun query-replace-ruby-18-to-19-stayle-hash (&optional delimited start end)
  "Rubyの 1.8 スタイルの Hash を 1.9 から導入されたスタイルへ確認しながら変更する ネストした hashには対応していない"
  (interactive)
  (query-replace-regexp ":\\([^ ]+\\) => \\([^ ]+\\)" "\\1: \\2" delimited start end))
```

という正規表現を指定しただけの Emacs Lisp も書いたけどみなかったことにしてください。
