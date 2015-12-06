---
date: 2015-12-06T21:50:42+09:00
title: "内包表記とPythonと… - #LT駆動 21"
tags: ["python", "Scala", "内包表記"]
---


[LT駆動開発21](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA21)でLTしてきました。
タイトル「内包表記とPythonと…Option」です。
PyCon Mini Hiroshima用に用意してたネタですが、参加できなかったので放出しました。
Pythonの内包表記でScalaのOptionのようなものをつくってみました。

Pythonの内包表記はScalaではfor式で表現ができます。
Scalaではリスト以外のものでもfor式が使えます。
代表格として`Option`が上げられます。
ということで、PythonにもOptionクラスをつくり内包表記で利用できるようにしてみました。
スライドに登場するように内包表記に対応するために、`__iter__`と`next`メソッドを実装をしています。

```python
class Option:
    def __init__(self, value):
        self.value = value
    def __iter__(self):
        return self
    def next(self):
        if self.value == None:
            raise StopIteration
        else:
            ret = self.value
        self.value = None
            return ret
    def __str__(self):
          if self.value == None:
               return "Nothing"
          else:
               return "Some(%d)" % self.value

def add(x, y):
  return [ x_+ y_ 
    for x_ in x 
    for y_ in y
  ]

add(Option(1), Option(2))    # => [3]
add(Option(1), Option(None))  # => []
add(Option(None), Option(2))  # => []
```

LT駆動はゆるふわに個々が学んだことを発表していてとても楽しいです。気軽に参加してみてください。

<script async class="speakerdeck-embed" data-id="c891be7c13ac4b8b8076e321e11b352d" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
