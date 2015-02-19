---

title: "Mac で使える git mergetool をいろいろ試してみる - p4merge"
date: 2013-06-29T18:58:00+09:00
comments: true
categories: ["git","mac","p4merge"]
---

[Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/) の続きです。

[p4Merge](http://www.perforce.com/product/components/perforce-visual-merge-and-diff-tools) を紹介します。

先に問題点などを書いておくと、日本語が利用されていると表示がずれてしまいます。
Qt が利用されているので Cocoa アプリに比べると動きは悪いですが、インターフェイスは使いやすいです。

インストールには、サイト下部の Download now をクリック後にOSなどを選択してダウンロードします。
ダウンロード後は`/Application` に配置します。

設定は以下のコマンドでできます。

```
git config --global mergetool.p4merge.path /Applications/p4merge.app/Contents/MacOS/p4merge
git config --global mergetool.p4merge.keepTemporaries false
git config --global mergetool.p4merge.trustExitCode false
```

あとはコンフリクトした際に

```
git mergetool -t p4merge
```

と入力すると利用できます。

`git mergetool` だけで起動できるようにしたい場合は

```
git config --global merge.tool p4merge
```

としておくとよいです。


起動すると下記のような画面です。

![p4merge画面](/images/p4merge.png)

画面中部に 左から

* マージしたいブランチのファイルの内容
* 枝わかれした時のファイルの内容
* 現在のファイルの内容

になってます。他とは左右が逆なので注意が必要です。

画面下部にはマージ結果の情報が出ています。
下部の右側のボタンを押すことでどちらの変更を使うか選択できます。
両方を選ぶことはできませんでした。
また、選択後に編集することもでき、アンドゥもすることができます。

ツールバー一番端で最初からやりなおすこともできて、良いと思いました。

編集後は p4merge を終了することで、次のファイルへ移行として同じ作業を繰返します。

マージする前のファイルが .ファイル名に `.orig` という文字が付加されて残っています。

日本語を使うと表示がずれてしまうので、日本語さえさえ使わなければ使いやすいツールでした。

# 関連

* [Mac で使える git mergetool をいろいろ試してみる - 準備編](/blog/2013/06/26/git-mergetool/)
