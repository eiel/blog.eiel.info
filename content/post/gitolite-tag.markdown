---

title: "gitolite で tag への権限付与"
date: 2013-08-23T22:32:00+09:00
comments: true
tags: ["gitolite"]
---

[gitolite](https://github.com/sitaramc/gitolite) でリポジトリを管理しております。
git の使い方わかってない子もいるのでわりと push できるブランチを制限してるリポジトリがあります。
タグを push しようとしたら拒否された。

というわけで許可を与える方法を調べた。

```
RW+    refs/tags/    = eiel
```

`refs/tags/` にたいして行えばよいみたいです。

補足しとくと形式は

```
[許可属性]  [ブランチなど正規表現]  = [ユーザまたはグループ]
```

という感じです。


### 参考

* [gitolite - GitHub](https://github.com/sitaramc/gitolite)
