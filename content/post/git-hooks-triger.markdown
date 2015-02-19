---

title: "git push 時に発生する update hookを起動するワンナイナー"
date: 2012-11-20T15:41:00+09:00
comments: true
categories: ['git']
---

git の hooks の update hook の動作確認するのに使ってたワンナイナー。
update というのは リモートリポジトリに push したときに リモートで起動するスクリプト。

```
$ git commit --amend --no-edit; git push --force
```
`git commit --amend` を使用してコミットの日付を変更しておけば新しい commit として認識してくれるので、--force オプションで無理矢理 push してやる。
コミットが新しくなるので hook が起動してくれます。
`git push` の部分は適宜好みに合わせて。
