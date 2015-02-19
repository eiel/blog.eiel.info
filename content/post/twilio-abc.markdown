---

title: "Twilio を使って、着信でモールス信号してみた。"
date: 2014-03-15T02:54:00+09:00
comments: true
tags: ["twilio","ネタ"]
---

[Twilio API 勉強会](http://twiliomeetup.doorkeeper.jp/events/9078)に遊びにいった。

勉強会では基本的なことを学んだ。
電話をかけたときの動作を登録したり、電話をかけたりしました。

せっかくなので、なにか作ってみることにした。

以下、完全にネタです。実用性皆無です。
あとサービスにどれくらい負荷がかかるのかよくわからないので、試す場合はほどほどにしましょう。
たぶん、もう二度と試さない。

着信する長さが一応調整できるのでモールス信号してみました。

コードは以下の感じ。

```ruby
require 'twilio-ruby'
require 'morse'

account_sid = 'account_sid を設定する'
auth_token = 'auth_tokenを設定する'
@client ||= Twilio::REST::Client.new account_sid, auth_token

TWILIO_NUMBER = 'Twilio で作成した電話番号を登録する'
MY_NUMBER = '自分の電話番号を設定する'

def call(n)
  call = @client.account.calls.create(
    from: TWILIO_NUMBER,
    to: MY_NUMBER,
    url: 'http://example.com/',
    timeout: n,
  )
  puts "create call #{call.sid}"
  loop do
    call = @client.account.calls.get(call.sid)
    puts "call status #{call.status}"
    case call.status
    when 'no-answer', 'completed'
      sleep(1)
      break
    when 'failed','canceled'
      break
    when 'queued','ringing','in-progress','busy'
      sleep(0.5)
    end
  end
end

morse = Morse.encode(ARGV[0])
puts "word: #{ARGV[0]}"
puts "morse: #{morse}"
morse.each_char do |c|
  case c
  when '.'
    call(1)
  when '-'
    call(3)
  when ' '
    sleep(1)
  end
end
```

使い方

```
$ ruby 作成したファイル hello
```

実行例

動画とりたいけど、とってない。

```
ruby twilio-sample.rb a
word: a
morse: .-
create call CA9ec2867270b920fcebbe47583ee0b9d2
call status ringing
call status ringing
call status ringing
call status ringing
call status no-answer
create call CAdf11adbe89a3d995369e97531f4b22f9
call status ringing
call status ringing
call status ringing
call status ringing
call status ringing
call status no-answer
```

簡単に解説。

まず、call メソッド。引数 n でn秒間着信する電話をかけます。

仕組みは電話をかけると sid が振られます。
sid をつかって twilio に問い合せると状態が取得できます。
コールが終わっているか確認し、終わっていたら処理が戻るようにしています。こうすることで call メソッドを並べていけるようにしました。
キューに入れることができればこんなことしなくていいのに…。
よくわからなかった。

あとは好きな文字をモールス符号に変換して、あとは短いところを 1秒、長いところを3秒になるようにしてみました。

そんなに応答速度がいいわけではないので、時間かかるし、コール時間も安定しない。サービスにどれくらい負荷がかかるのかわからないので、良い子は何度も何度も実行しないほうがいいと思う。

### 蛇足

終わった後、おうちに帰ってなんか少しだけネタなことしてみようと思い、電話をかける部分で遊ぶことにした。

通話してしまうと、電話代かかりますしね。(トライアル期間なので大丈夫ですが)

しかし、ローカルで遊ぼうと思うとできることが限界があります。
Twilio からローカルマシンにコールバックする方法が基本的にないからです。
コールバックを受けるには公開サーバを用意する必要があります。

静的ファイルだけでも繰り返しや分岐はできます。
しかし、状態が保存できないため、表現力に限界が。

電話をかけた情報などはローカルでも取得可能なので、電話をかけることで遊ぶことにしました。

本来の使い方とは違うし無駄に負荷がかかるような気がするので、こういった遊びはほどほどにしましょう。

一応、規約には目を通してみたけど、極端に負荷をかけなければ大丈夫そう。

### 参考文献

* [Twilio Docs - API REST Call](https://jp.twilio.com/docs/api/rest/call)
* [Twilio Docs - API REST Making Calls](https://jp.twilio.com/docs/api/rest/making-calls)
