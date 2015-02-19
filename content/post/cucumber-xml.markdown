---

title: "cucumber で表示した画面がXMLを出力しているか確認する"
date: 2012-11-07T15:27:00+09:00
comments: true
tags: ["rails", "cucumber"]
---

rspec でマッチャーがあればよいのですが、とりあえず心当たりがなかったので、適当にごまかしました。良いgemがあれば紹介して欲しいです。

page.source が サーバからの出力を返してくださるので、これを Nokogiri で parse させてエラーがないかどうかで確認しました。

```ruby
ならば /XMLを出力する/ do
  errros = Nokogiri::XML(page.source).errors
  expect(errors).to be_empty
end
```

どうせなら下記のように書きたいですね。

```ruby
ならば /XMLを出力する/ do
  should render_xml
end
```

マッチャーを書いてみます。

```ruby
RSpec::Matchers.define :render_xml do
  match do |actual|
    Nokogiri::XML(actual.source).errors.empty?
  end
end
```

ほとんどそのままです。matcher つくるのは難しくないので気軽に作りたいです。

少しだけ解説。
y
cucumberの中では subject を省略した場合は page になります。なので、

```ruby
ならば /XMLを出力する/ do
  page.should render_xml
end
```

と書いたのと等しいです。なので、acutual には page オブジェクトがバインドされていますので、そこから source を取り出してチェックします。page オブジェクトには html というメソッドが存在しますが、ブラウザが解釈したあとのDOMをdumpしたような感じになってるので期待通りの動きをしませんでした。
