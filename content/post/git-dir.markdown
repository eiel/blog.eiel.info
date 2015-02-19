---

title: "Gitリポジトリを直接動かして変更を検知 - QA@ITで遊んでる"
date: 2013-06-05T13:26:00+09:00
comments: true
categories: [git]
---

朝早く起きることができたら、[QA@IT](http://qa.atmarkit.co.jp/)で、遊んでいます。
ざっと未解決の質問を見て、すぐわかりそうなものを試して解答をします。

今日は 「[rails new project -d postgresql を指定した時に変更される部位](http://qa.atmarkit.co.jp/q/2970)」というのがあって、「試せばいいやーん」と思ったので、すぐに手を動かしてみました。

`rails new project` と `rails new project -d postgresql`  の違いについて。

だいたい以下の操作をすれば違いはわかります。

```bash
rails new project
mv project project_sqlite3

rails new project -d postgresql
diff -ru project_sqlite3 project
```

とすれば違いは簡単にわかります。

new の引数は同じ名前にしないと、余計な差分ができてしまうので、作成した後に リネームしています。

### 折角なので Git を絡めてみた

Git のリポジトリって直接移動してもそのまま使えるんだぜ!

```bash
rails new project
mv project project_sqlite3

cd project_sqlite3
git init
git add .
git commit -m 'initial'
cd ..

rails new project -d postgresql

mv project_sqlite3/.git project
cd project
git diff
```

という、感じの解答をしてみました。

`.git` を移動をしてるのが味噌です。

リポジトリに対する物理的なワークツリーが変わっても簡単に認識してくれます。
このようにGitの理解が深まってくると応用力がついてきて、「subversion より Git 楽しいなぁ」と、個人的にはなります。

### 別解

git には リポジトリを指定するオプション `--git-dir` があります。
なので以下のような方法もあります。

```bash
rails new project
mv project project_sqlite3

cd project_sqlite3
git init
git add .
git commit -m 'initial'
cd ..

rails new project -d postgresql

cd project
git --git-dir=../project_sqlite3/.git diff
```


また、ワークツリーを指定するオプション `--work--tree`もあります。

```bash
rails new project
mv project project_sqlite3

cd project_sqlite3
git init
git add .
git commit -m 'initial'
cd ..

rails new project -d postgresql

cd project_sqlite3
git --work-tree=../project diff
```

Git 楽しいですねー。

## 戯言

QA@ITのサイト内ランクが100位切りました。ユーザすくないですな…。

## 関連

* [このコード書いた誰だよ! そんな時の Git Log -S でもしてみよう](/blog/2013/06/04/git-log-s/)
* [Git Push 時に発生する Update Hookを起動するワンナイナー](/blog/2012/11/20/git-hooks-triger/)
