---

title: "Rails の rake notes というタスクをいまさらしった。"
date: 2013-01-19T11:35:00+09:00
comments: true
tags: ["rails"]
---

[Rails Guide](http://guides.rubyonrails.org/command_line.html) 読んでたら `notes` というタスクがあるのを知りました。

コードのコメントに `TODO` とか `FIXME`, `OPTIMIZE` といったコメントをみつけて表示してくれます。
`grep すればいいや`とか思いますが、それなりの便利そうです。jenkins なんかで回してレポートに出すのもよいかもしれません。

```
$ roke notes
app/controllers/admin/users_controller.rb:
  * [ 20] [TODO] any other way to do this?
  * [132] [FIXME] high priority for next deploy
 
app/model/school.rb:
  * [ 13] [OPTIMIZE] refactor this code to make it faster
  * [ 17] [FIXME]
```

`notes:custom` というタスクもあるようです。

### もう少しつっこんでみよう

タスクの[ソースコード](https://github.com/rails/rails/blob/master/railties/lib/rails/tasks/annotations.rake)がここになります。
`notes:custom` では 環境変数 ANNOTATION で表示するキーワードを決められるようです。

`notes`の場合は "OPTIMIZE", "FIXME", "TODO" の3つか使用されていますね。
あと 動的にタスクを生成しているようなので`notes:todo` `optimize` `fixme` といったタスクも存在するようです。
