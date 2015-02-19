---

title: "Gitのソースコードでも読もうかな。準備編 - デバッガ"
date: 2012-12-22T11:06:00+09:00
comments: true
tags: ["git","コードリーディング","gdb"]
---

ソースコードよみたいなー。よみたいなーってことで、環境を整えていきたいと思います。[ひらメソッド](http://hira-consulting.com/wiki/index.php?2005_6_23%A5%AB%A1%BC%A5%CD%A5%EB%BA%C2%C3%CC%B2%F1%AD%A1)にも挑戦したい。

静的によむのも良いのですが、答え合わせができないと遠まわりです。デバッガを使って答え合わせできると効率がよいです。なので、ソースコードをDLして、動作確認するところまで試してみましょう。

Gitのソースコードは [https://github.com/git/git](https://github.com/git/git) とかにあります。

cloneしてみましょう。

```bash
$ git clone git://github.com/gitster/git.git
$ cd git
```

別にどのバージョンにしてもいいのですが、自分が使っているバージョンでコードリーディングしていきたいと思います。

```bash
$ git --version
git version 1.8.0.2
$ git checkout v1.8.0.2
```

これで v1.8.0.2 のソースコードになっています。デバッグ情報をもった状態でビルドしてみましょう。

```bash
$ make -d
```

`-d` をつけることでOKです。

しばらく待つとバイナリが生成されますので、`gdb` をつかってみましょう。

```bash
$ gdb git
(gdb)
```

これで `gdb` が起動できます。プロンプトに (gdb)と表示されます。
この状態で `r` を入力すると実行ができます。

```bash
(gdb) r
usage: git [--version] [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p|--paginate|--no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           [-c name=value] [--help]
           <command> [<args>]

The most commonly used git commands are:
   add        Add file contents to the index
   bisect     Find by binary search the change that introduced a bug
   branch     List, create, or delete branches
   checkout   Checkout a branch or paths to the working tree
   clone      Clone a repository into a new directory
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   fetch      Download objects and refs from another repository
   grep       Print lines matching a pattern
   init       Create an empty git repository or reinitialize an existing one
   log        Show commit logs
   merge      Join two or more development histories together
   mv         Move or rename a file, a directory, or a symlink
   pull       Fetch from and merge with another repository or a local branch
   push       Update remote refs along with associated objects
   rebase     Forward-port local commits to the updated upstream head
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index
   show       Show various types of objects
   status     Show the working tree status
   tag        Create, list, delete or verify a tag object signed with GPG

See 'git help <command>' for more information on a specific command.

Program exited with code 01.
```

ブレークポイントを利用して停止させてみましょう。

```bash
(gdb) b main
(gdb) r
Breakpoint 1, main (argc=1, argv=0x7fff5fbff1a8) at git.c:535
535             startup_info = &git_startup_info;
```

main 関数で止まるようにしてみました。
そのあと実行すると `git.c` の 535行目で停止していることがわかります。

一行づつ実行していくには `n` を使います。

```bash
(gdb) n
537             cmd = git_extract_argv0_path(argv[0]);
```

ひたすら `n` を入力していくとプログラムが1行づつ動作していくことがわかります。

`p` を使うと 変数の中身が見れます。

```bash
(gdb) p argv[0]
$1 = 0x7fff5fbff390 "/Users/eiel/src/git/git"
```

とっても簡単ですね。上手いこと利用してソースコードを読んでいきましょう。

終了するには `q` を使います。

```bash
(gdb) q
The program is running.  Exit anyway? (y or n) y
```

gdbのもっと詳しい使い方はぐぐってみましょう。
`emacs` などと連携して使うともっと便利になると思います。
