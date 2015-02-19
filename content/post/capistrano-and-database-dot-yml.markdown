---

title: "database.ymlが.gitignoreに入っている環境でのcapistranoを使ったデプロイ"
date: 2012-08-31T11:15:00+09:00
comments: true
categories: [rails,capistrano]
---

[2012-09-20日追記] 以下の記事の内容ですが、gemが用意されてました。

https://github.com/amfranz/capistrano_database_yml


Railsプロジェクトで、ひとりで開発している場合は除いて、`.gitignore`に`config/database.yml`を追加することがよくよくあります。この場合、`config/database.yml`をどこかのタイミングで作ってやらなければなりません。この対処法は[この記事](http://www.simonecarletti.com/blog/2009/06/capistrano-and-database-yml/)で説明されてます。

## 概略

英語だったりするので簡単に説明をかいておきます。
前述の記事の作業を行うと`cap`コマンドにふたつのタスクが追加されます。

* cap db:setup
* cap db:symlink

`db:setup`は`shared_path`に`config/database.yml`に生成してくれます。すでにあると上書きされるので注意が必要です。
`db:symlink`は `db:setup`で生成した`config/database.yml`へsymlinkを貼ります。なので、`#{shared_path}/config/database.yml`手で編集しておけば良いということになります。

## 試したときに困ったことなど
記載されているコードをコピーして、`capistrano_database.rb`を作成するのですが、このファイルをどこに作成すべきなのか記述されていないので、`lib`上に作成しました。
`lib`は`LOAD_PATH`が通ってないのに気付かなくて迷惑をかけました。
`config/deploy.rb`に `require "capistrano_database"`とするところを`require "./lib/capistrano_database"`として回避しました。
ところが、cucumberの実行時のオプションに`--require lib`をつけているせいだと思いますが、cucumberの実行時に読み込まれてしまい不具合がでてしまったので、

```ruby
Capistrano::Configuration.instance.load do
  ....
end
```

の部分を

```ruby
if instance = Capistrano::Configuration.instance
  instance.load do
    ...
  end
end
```
のように変更して回避しました。
