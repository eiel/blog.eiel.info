---

title: "Git で現在チェックアウトしているコミットのID"
date: 2014-09-04T14:57:00+09:00
comments: true
categories: ["git"]
---

稀に今チェックアウトしてるところのコミットIDを知りたいときがある。
ぐぐったら下記のページがあったけど、grep やら awk やらつかっててずるい気がした。

* [gitリポジトリのリビジョン(コミットID)を取得するワンライナー | tomotaka-itoの日記](http://tmtk.org/blog/2011/05/164)

```
git log |grep '^commit' |head -1|awk '{print $2}'
```

git コマンドだけで完結できる気がするので help を読んだりした。

以下に落ちついた。

```
git show -s --format=%H
```

```
git log -n 1 --format=%H
```

### 関連

* [このコード書いた誰だよ! そんな時の git log -S でもしてみよう - そんなこと覚えてない](http://blog.eiel.info/blog/2013/06/04/git-log-s/)
* [Git で特定のコミットがどのタグに含まれているか確認する - そんなこと覚えてない](http://blog.eiel.info/blog/2014/05/28/cotains-commit-in-tags/)
