---

title: "Mac で使える git mergetool をいろいろ試してみる - ediff"
date: 2013-06-29T19:27:00+09:00
comments: true
categories: ["git","mac","emacs","ediff"]
---


[Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/) の続きです。

ediff は [Emacs](http://www.gnu.org/software/emacs/) に添付されているマージツールです。
いつから添付されているのか調べてないですが最近のEmacsであれば標準で使えるはずです。

個人的には一番使いやすいと感じています。

`git mergetool` から ediff を起動するのはややめんどくさいです。

[magit](https://github.com/magit/magit)を使用していれば、コンフリクトしているファイルにカーソルを合わせて `e` を押すと起動できます。こちらのほうは設定いらずで楽ちんでした。

どうしても `git mergetool` から使いたい場合は [git mergetoolでEmacsのediff-merge-files-with-ancestorを呼び出す - 工夫と趣向と分別と。](http://d.akinori.org/2012/07/23/git-mergetool%E3%81%A7emacs%E3%81%AEediff-merge-files-with-ancestor%E3%82%92%E5%91%BC%E3%81%B3%E5%87%BA%E3%81%99/) を参考にするとできました。

具体的には上記の記事で紹介されてる [emacsc](https://github.com/knu/emacsc) をclone してきて設定しました。
個人的な使い方の問題で emacsclient への引数を付加したかったので少しいじりました。

![ediff の画面](/images/ediff.png)

ediff を起動するとこのような画面になります。
配色設定を細かくやってないのでみづらいのは気にしないでください。

左が現在のブランチのファイルで、右がマージしようとするブランチのファイルになります。
下部がマージ結果になります。下部は直接編集することもできます。

`a` を入力すると 左側を選択できて、 `b` を入力すると右側を選択できて、下部に反映されます。

差分は2行あるのに、まとまってしまって不便に感じますが、どちらをベースするかという選択だと考えると良いことがわかりました。
その後 `!` を入力すると細かく差分が表示され `n` や `p` で競合箇所を移動して `a` や `b` で選ぶことができます。

編集が終了したら `q` で終了できます。 magit から起動した場合は ステージングされないので注意してください。
再度 `e` を入力するとやりなおすこともできます。

emacs 上で動くとカスタマイズが気軽にできるところも嬉しいし、なによりキーボードで操作しやすいです。
あえて問題を上げるなら ediff の設定がしてある配色を使わないと見にくいのと、emacs ユーザでない場合は覚えることが多すぎるということでしょう。

# 関連

* [Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/)
