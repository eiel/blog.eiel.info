---

title: "もっと楽ができた。bundle init で作成したプロジェクトの rake task"
date: 2012-11-27T00:23:00+09:00
comments: true
categories: ["ruby","bundle"]
---

以前 [勢いでhiroshimarbというgemを作った。反省する気なんてあんまりない。](/blog/2012/09/02/hiroshimarb-gem/)という記事で gem の リリースをする方法を書いたのですが、

> bundle gem で作られた rake タスクも見てあげると良いかもしれません（rake releaseだと push しつつ tag も切ってくれたりする）。(via @sugamasao)
>
> https://twitter.com/sugamasao/status/268286597110312960

というコメントを頂いてました。
なので、調べました。

```bash
$ rake -T
rake build    # Build hiroshimarb-0.1.4.gem into the pkg directory
rake install  # Build and install hiroshimarb-0.1.4.gem into system gems
rake release  # Create tag v0.1.4 and build and push hiroshimarb-0.1.4.gem to Rubygems
```

`rake release` で git でタグをつくりつつ、rubygems.org に uploadしてくれました。
生成したgemは `pkg` ディレクトリ内に保存されます。対した作業ではないですが、バージョンを入力する手間が省けて素敵ですね。

taskの中身は Rakefileが
```bash
$ cat Rakefile                                                              (gi#!/usr/bin/env rake
require "bundler/gem_tasks"
```
ということで [gem_task.rb](https://github.com/carlhuda/bundler/blob/master/lib/bundler/gem_tasks.rb) をみてみましょう。
```ruby
require 'bundler/gem_helper'
Bundler::GemHelper.install_tasks
```

[Bundler::GemHelper.install_tasks](https://github.com/carlhuda/bundler/blob/master/lib/bundler/gem_helper.rb#L13-L15)が呼ばれてます。
```ruby
def install_tasks(opts = {})
  new(opts[:dir], opts[:name]).install
end
```
install_tasksはインスタンスを生成して installすることがわかります。

つづいて インスタンスを生成するので、initilaizeです。
[Bundle::GemHelper#initialize](https://github.com/carlhuda/bundler/blob/master/lib/bundler/gem_helper.rb#L26-L33)では gemspecを読み込んでいるようです。なんとなくしかみてません。
```ruby
def initialize(base = nil, name = nil)
  Bundler.ui = UI::Shell.new
  @base = (base ||= Dir.pwd)
  gemspecs = name ? [File.join(base, "#{name}.gemspec")] : Dir[File.join(base, *}.gemspec")]
  raise "Unable to determine name from existing gemspec. Use :name => 'gemname' in #install_tasks to manually set it." unless gemspecs.size == 1
  @spec_path = gemspecs.first
  @gemspec = Bundler.load_gemspec(@spec_path)
end
```

そして install で rake タスクの生成をしています。
[Bundle::GemHelper#install](https://github.com/carlhuda/bundler/blob/master/lib/bundler/gem_helper.rb#L35-L52)

```ruby
def install
  desc "Build #{name}-#{version}.gem into the pkg directory."
  task 'build' do
    build_gem
  end

  desc "Build and install #{name}-#{version}.gem into system gems."
  task 'install' do
    install_gem
  end

  desc "Create tag #{version_tag} and build and push #{name}-#{version}.gem to Rubygems"
  task 'release' do
    release_gem
  end

  GemHelper.instance = self
end
```

そしてついに gem の生成。
[Bnudle::GemHelper#build_gem](https://github.com/carlhuda/bundler/blob/master/lib/bundler/gem_helper.rb#L54-L63)

```ruby
def build_gem
  file_name = nil
  sh("gem build -V '#{spec_path}'") { |out, code|
    file_name = File.basename(built_gem_path)
    FileUtils.mkdir_p(File.join(base, 'pkg'))
    FileUtils.mv(built_gem_path, 'pkg')
    Bundler.ui.confirm "#{name} #{version} built to pkg/#{file_name}."
  }
  File.join(base, 'pkg', file_name)
end
```
pkgディレクトリに生成している様子が見えます。


つづいて `install_gem`

[Bundle::GemHelper#install_gem](https://github.com/carlhuda/bundler/blob/master/lib/bundler/gem_helper.rb#L65-L70)
```ruby
def install_gem
  built_gem_path = build_gem
  out, _ = sh_with_code("gem install '#{built_gem_path}'")
  raise "Couldn't install gem, run `gem install #{built_gem_path}' for more detailed output" unless out[/Successfully installed/]
  Bundler.ui.confirm "#{name} (#{version}) installed."
end
```
`build_gem` を呼びだして、生成した上で insntall するだけのようです。

最後に `release_gem`
[Bundle::GemHelper#release_gem](https://github.com/carlhuda/bundler/blob/master/lib/bundler/gem_helper.rb#L72-L77)
```ruby
def release_gem
  guard_clean
  built_gem_path = build_gem
  tag_version { git_push } unless already_tagged?
  rubygem_push(built_gem_path)
end
```

`guard_clean`というのは変更があるかどうかを `git diff` を利用して確認してるようです。変更があれば例外が飛ぶようです。
そのあと `build_gem`で gemを生成し、
tag を打った上で `git push`し、
rubygems に pushしてくれるようです。

おー。便利ですね。

なんとなくソースコードを追う手順も一緒に書いてみました。参考になれば幸いです。
