---

title: "Mac で使える git mergetool をいろいろ試してみる - Vimdiff2"
date: 2013-07-03T20:08:00+09:00
comments: true
tags: ["git","vim","mac"]
---

[Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/) の続きです。

Vimdiff2を紹介してみます。
emacs使いですが、
vimさえ入っていれば、設定不要で、「気軽に使えるかなぁ。」という目論見です。
vimdiff2 とかいてみましたか、vim の起動方法が違うだけのような感じでした。
よくわかりません。
vimdiff でも起動できます。

設定は不要で、

```
git mergetool -t vimdiff2
```

とすると起動できます。

また、常に vimdiff2 を利用したい場合は

```
git config --global --global merge.tool vimdiff2
```

としておけばよいです。

ヘルプを見るには vim を起動した状態で

```
:h vimdiff
``

で help が出せました。
[pecosantoyobe](http://twitter.com/pecosantoyobe)に教えてもらいました。
ありがとうございます。

![vimdiff の画面](/images/vimdiff2.png)

画面左が現在のブランチのファイルの内容で、画面右がマージするブランチのファイル内容です。
中央にはコンフリクトしたファイルの内容がでており、修正することができます。

次のコンフリクト場所に移動するには `]c` で移動することができます。
前のコンフリクト場所に移動するには `[c` で移動することができます。

コンフリクト場所に移動して、`:diffget L` と入力すると 左の内容を取り込むことができ、`:diffget R` と入力すると右の内容を取り込むことができます。

移動も含めてショートカットを用意すると、とても便利そうです。

終了の仕方がよくわかりませんでした。
`ZZZZZZ` として終了しました。
もっとよい方法があるような気がします。

終了すると次のファイルが自動的に開きました。
とても便利でした。

なんとなく emacs に渡せないときに使いたいと思います。

# 関連

* [Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/)
