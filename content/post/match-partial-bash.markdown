---

title: "部分一致 - Bash"
date: 2012-11-25T11:23:00+09:00
comments: true
categories: ["Bash"]
---

シェルスクリプトで部分一致を確認したい場合どうするんだろーっと思って考えた結果 grep の終了ステータスで確認すればいいんじゃないかということで。以下のように書いた。

```
search_term="openssl"
target="openssl-1.0.0"

if echo $target | grep "$search_Term" > /dev/null; then
  echo "goro"
fi
```

せっかくなので簡単に解説

shellの if は終了ステータスに応じて分岐します。
終了ステータスは $? に代入されています。

`echo $target | grep "$search_Term" > /dev/null; echo $?`

とすればどんなステータスを返すのか確認できます。
grep は match しなければ 終了ステータス 1 を返すので 確認したい文字を流し込んでチェックして、余計な内容を表示しないように /dev/nullにリダイレクトしてます。

もっと良い方法があれば教えていただきたい。
