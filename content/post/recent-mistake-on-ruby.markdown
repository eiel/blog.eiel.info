---

title: "最近やった Ruby でのミス - rspec の expect で 空白に括弧"
date: 2013-08-26T01:32:00+09:00
comments: true
categories: ["ruby"]
---

Ruby かいてて些細なミスでハマったことを書いておきます。その2。

```ruby
describe 'Hoge' do
  it do
    expect ('hoge').to eq('hoge')
  end
end
```

`expect ('hoge')` のところの括弧の前にスペースが入っているのが問題です。
正しくは

```ruby
describe 'Hoge' do
  it do
    expect('hoge').to eq('hoge')
  end
end
```

`'hoge'に``to` というメソッドがない、というエラーが出るので気づくのですが `+` がないというエラーになっててハマった。

### 解説

`expect('hoge').to eq('hoge')` は `expect('hoge').to(eq('hoge'))` と等価です。
`expect ('hoge').to eq('hoge')` は `expect('hoge'.to(eq('hoge')))` と等価です。

`to` は expect の戻り値に対して使うメソッドです。
空白をいれてしまうと、expect ではなく 'hoge' に対し `to` を呼んでしまいます。

### 関連記事

* [最近やった Ruby でのミス - カンマで改行](/blog/2013/06/24/recent-mistake-on-ruby/)
