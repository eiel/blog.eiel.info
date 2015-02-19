---

title: "Mac で使える git mergetool をいろいろ試してみる - 準備編"
date: 2013-06-26T19:08:00+09:00
comments: true
categories: ["git","Mac"]
---

この記事は [すごい広島 #6](http://great-h.github.io/events/event-006.html) での活動の一部です。

Git で branch をマージしたときにコンフリクトが起きると、これを解消する必要があります。テキストエディタでがんばるのはつらいこともありますよね。

そんなとき、マージするためのツールを使いたい場合もあります。
Git に `git mergetool` というコマンドがあって、設定しておいたツールを起動することができます。
同様に 差分を見るのにGUIツールを使いたい場合などには `git difftool`というコマンドもあります。

基本的には Mac で使えるものを紹介しますが、マルチプラットフォームのもあるので、別の環境でも使えるものもあります。

## 試したツール

* [opendiff - 無料 Xcode に添付されている](/blog/2013/06/26/git-mergetool-opendiff/)
* [p4merge - 無料 Qt](/blog/2013/06/29/git-mergetool-p4merge/)
* [ediff(emacs) - 無料](/blog/2013/06/29/git-mergetool-ediff/)
* [Kaleidoscope - 有料 - Cocoa アプリ (Mac Only)](/blog/2013/06/29/git-mergetool-kaleidoscope/)
* [deltawalker - 有料 マルチプラットフォーム](/blog/2013/07/03/git-mergetool-deltawalker/)
* [vimdiff2(vim) - 無料](/blog/2013/07/03/git-mergetool-vimdiff2/)
* [tkdiff - 無料 マルチプラットフォーム](/blog/2013/07/03/git-mergetool-tkdiff/)
* [xxdiff - 無料 マルチプラットフォーム](/blog/2013/07/03/git-mergetool-xxdiff/)
* [Araxis Merge - 有料 (Winあり)](/blog/2013/07/10/git-mergetool-araxis/)

# いろいろ試すために用意したもの

いろいろ試すのになるべく楽をしたいので以下のリポジトリを用意しました。

* [Github eiel/git-merge-sandbox](https://github.com/eiel/git-merge-sandbox)

このリポジトリを clone して `$ bin/restart` を実行するとそのまま `$ git mergetool` 実行できる状態で、すでにコンフリクトした状態になるようにしています。

試すのに**簡単にやりなおせる**様にするのは大事だな、と最近感じています。

コンフリクトするファイルは

* sample.txt
* sample2.txt

です。

コンフリクトを解消する練習に利用してみてください。

# 試すツールを探した方法

`$ git mergetool --tool-help` の実行結果に使えるツールの一覧が出てくるので、片っ端に調べました。

次に google さんに聞いて、いくつか探しました。

以下のツールは環境作るがめんどくさいので試しませんでした。

* meld
* gvimdiff2

meld は GTK がいるので試してませんでした。

gvimdiff2 は 実質 vimdiff2 と同じなので試しませんでした。

各ツールの詳細に続く

# まとめ

一通り試した感想を。

Emacs や Vim を使いなれているのであれば、 ediff,vimdiff2 が第一候補になるでしょう。

お金があるなら Kaleidscope が表示が綺麗でよさそうです。
デザイナーさんに薦めるならこれが一番だと思います。

フリーのGUIツールでは p4merge がよさそうでした。
