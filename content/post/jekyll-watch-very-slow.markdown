---

title: "pygements を利用してると jekyll serve --watch のファイル生成が遅いらしい"
date: 2013-08-30T22:25:00+09:00
comments: true
categories: ["Jekyll"]
---

以下の質問を受けた。

<blockquote class="twitter-tweet"><p>反映されるのに、なんでこんなに時間がかかるのじゃ。&#10;Jekyll on Vimeo&#10;<a href="https://t.co/RrU2ABliHw">https://t.co/RrU2ABliHw</a></p>&mdash; Terasawa Shuuhei (@shuuheyhey) <a href="https://twitter.com/shuuheyhey/statuses/373403280669294593">August 30, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

`--watch` を使うと `jekyll build` の部分が異常に遅くなるらしい。
初回は遅くないのですが、ファイル変更を検知した時が遅い。

<blockquote class="twitter-tweet"><p><a href="https://twitter.com/shuuheyhey">@shuuheyhey</a> pygments: falseにすると速いんですけど、それだとシンタックスハイライトが効かないという…。</p>&mdash; Hideki Abe (@_hideki_a) <a href="https://twitter.com/_hideki_a/statuses/373420587772694530">August 30, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

pygments を有効にしていると遅くなるらしい。

`--watch` を使うとファイルの生成する部分がメインスレッドでないのがあやしい。pygements は `popen` とかつかって実行されるっぽいし。
とはいえ、原因を特定するまではいけなかった。

仕方ないので無理矢理ごまかす方法を考えた。

以下のような `Rakefile` を用意してみた。

```ruby
require 'directory_watcher'
require 'jekyll'

desc 'preview'
task preview: [:watch]  do
  sh 'bundle exec jekyll serve'
end

task :watch do
  options = Jekyll.configuration({})
  source = options['source']
  destination = options['destination']

  dw = DirectoryWatcher.new(source, :glob => Jekyll::Command.globs(source, destination), :pre_load => true)
  dw.interval = 1

  dw.add_observer do |*args|
    t = Time.now.strftime("%Y-%m-%d %H:%M:%S")
    print Jekyll.logger.formatted_topic("Regenerating:") + "#{args.size} files at #{t} "
    sh 'bundle exec jekyll build'
    puts  "...done."
  end
  dw.start
end
```

ほとんど `Jekyll::Commands::Build` からもってきた。
ファイルの変更を検知して `jekyll build` を実行しています。

問題が起きる最小セットをつくってのJekyll 本家に Issue を作成したいと思う。
### おまけ


* [ローカルサーバ起動と同時にブラウザで開く。 - Jekyll とかで。](/blog/2013/08/28/browse-open-when-rake-preview/)

で紹介したのを組み合せるとにこんな感じになります。

```ruby
require 'bundler/setup'
require 'thread'
require 'launchy'
require 'directory_watcher'
require 'jekyll'

desc 'preview'
task preview: [:watch]  do

  Thread.new do
    sleep 2
    Launchy.open 'http://localhost:4000/'
  end

  sh 'bundle exec jekyll serve'
end

task :watch do
  options = Jekyll.configuration({})
  source = options['source']
  destination = options['destination']

  dw = DirectoryWatcher.new(source, :glob => Jekyll::Command.globs(source, destination), :pre_load => true)
  dw.interval = 1

  dw.add_observer do |*args|
    t = Time.now.strftime("%Y-%m-%d %H:%M:%S")
    print Jekyll.logger.formatted_topic("Regenerating:") + "#{args.size} files at #{t} "
    sh 'bundle exec jekyll build'
    puts  "...done."
  end
  dw.start
end
```
