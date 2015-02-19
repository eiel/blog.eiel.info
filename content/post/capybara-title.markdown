---

title: "Capybaraでtitleタグの内容が取得できなくなってしまった。"
date: 2012-11-16T11:07:00+09:00
comments: true
categories: ["ruby","capybara","cucumber","rspec"]
---

[Capybara](https://github.com/jnicklas/capybara)を2.0にしたら動かなくなった [Cucumber](http://cukes.info/) の step がありました。titleタグ のtextをとる部分。visible でない要素のtextは取得できなくなったんでしょうか。
コードを追う余裕がなかったので、Nokogiriで対処した。

```ruby
target = find("title").text
expect(target).to eq(title)
```
を
```ruby
target = Nokogiri::HTML.parse(page.source).css("title").text
expect(target).to eq(title)
```
に書き換えました。

ちょっと無理矢理。

[Cucumber](http://cukes.info/)についてやりとりする仲間がいないので、titleタグのテキストの中身なんて確認しなくていいよ!とか、そういうい話ができないのが寂しいですね。
