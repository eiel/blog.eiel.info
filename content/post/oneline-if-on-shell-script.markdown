---

title: "シェルの if を1行でかく"
date: 2014-07-03T12:13:00+09:00
comments: true
tags: ["sh"]
---

[すごい広島 59](http://great-h.github.io/events/event-059.html) のメモ

[sensu](http://sensuapp.org/) の実験をしていて実行する command に if を含むスクリプトをかいていたのだけど、ちょっとはまったのでメモしとく。


以下のようなスクリプトを1行で書きたいとする。

```sh
HTTP_STATUS=`curl -w '%{http_code}' -s http://blog.eiel.info/ -o /dev/null`
if [ $HTTP_STATUS -eq 200 ]
then
  echo -n $STATUS
else
  echo -n $STATUS; exit 1
fi
```

1行で書くとこうなる。

```sh
HTTP_STATUS=`curl -w '%{http_code}' -s http://blog.eiel.info/ -o /dev/null`; if [ $HTTP_STATUS -eq 200 ]; then echo -n $STATUS; else echo -n $STATUS; exit 1; fi
```

then と else のすぐ後ろにはセミコロンがあってはいけないらしい。

整理すると

```sh
if [test]; then command1; else command2; fi
```

簡単に確認したいなら以下のような感じかしら。

```sh
$ if [ 1 -eq 1 ]; then echo true; else echo false; fi
true
```

```sh
$ if [ 2 -eq 1 ]; then echo true; else echo false; fi
false
```
