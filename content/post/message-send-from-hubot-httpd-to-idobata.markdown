---

title: "hubot で起動しているウェブサーバを経由して idobata へ投稿してみる"
date: 2014-05-27T16:24:00+09:00
comments: true
categories: ["hubot","nodejs","heroku","idobata"]
---

[hubot](https://hubot.github.com/) をつかって [idobata](https://idobata.io/) 用のBOTを作っていた。
[heroku](https://dashboard.heroku.com/apps) にホスティングして HTTPアクセスをすると idobata へメッセージが流れるようにしたい。

最終目的は CircleCI から WebHooks に登録しておいて、ビルドが終了したら通知したい、だったりするけど今回はそこはおいておく。

チャットへメッセージを送るには `robot#send` を呼べばよくて、第1引数が envelope で 第2引数以降が文字列。
idobata-adapter の場合は第1引数の message.data.room_id に投稿する ROOM_ID が必要になる。

* [参考 - hubot-idobata](https://github.com/idobata/hubot-idobata/blob/v0.0.3/src/idobata.coffee#L13-L14)

`robot.send_room 'hogehoge'` みたいな感じに投稿できるようにメソッドを加えるならこんな感じになった。

```
ROOM_ID = process.env.HUBOT_IDOBATA_DEFAULT_ROOM_ID

module.exports = (robot) ->
  robot.send_room = (msg) ->
    envelope = { message: { data: {room_id: ROOM_ID } } }
    @send envelope, msg
```

ROOM_ID を取得しないといけないので、登録したBOTに `room_id` を返すような機能を追加しました。

```
robot.respond /ROOM_ID/i, (msg) ->
  room_id = msg.message.data.room_id
  robot.logger.debug room_id
  msg.send room_id
```

ROOM_ID がわかったら 環境変数 HUBOT_IDOBATA_DEFAULT_ROOM_ID を設定しておく。
あとは `robot.router.get` とか `robot.router.post` をつかって処理を作成すればいよい。

```
robot.router.post "/hubot/ping", (req, res) ->
  robot.send_room 'pong'
```

ローカルで実行する場合は 8080 ポート。
heroku へは push するだけで使える。

/hubot/ping にアクセスするとチャットに pong と書き込みがされます。

hubot 自体のソースコードはそんなに長くないので、いじりながらソースよめばなんとか使えるようになりそうでした。

### 補足

robot.respond とか、使った場合は コールバック引数 msg があるのでこいつの `send` を利用するだけでチャットにかきこみができる。
msg は [Response](https://github.com/github/hubot/blob/v2.7.2/src/response.coffee) クラスのインスタンスでサーバからうけとった情報を message プロパティに保存されてて、この情報で envelope.message から取り出せる。

[Response#send は以下のように実装されていて](https://github.com/github/hubot/blob/v2.7.2/src/response.coffee#L21-L22)、

```
send: (strings...) ->
  @robot.adapter.send @envelope, strings...
```

@robot.adapter.send へいくので、それぞれの adapter の send メソッドをみてみればいい。

前述しているけど、[idobataの場合は](https://github.com/idobata/hubot-idobata/blob/v0.0.3/src/idobata.coffee#L13-L14)

```
send: (envelope, strings...) ->
  @_postMessage string, envelope.message.data.room_id for string in strings
```

となってて message.data.room_id が必要なことがわかる。

試したコードの全体をはっとく。
script ディレクトリに保存しておけばよいだけです。

```
ROOM_ID = process.env.HUBOT_IDOBATA_DEFAULT_ROOM

module.exports = (robot) ->
  robot.send_room = (msg) ->
    @send { message: { data: {room_id: ROOM_ID } } }, msg

  robot.respond /ROOM_ID/i, (msg) ->
    room_id = msg.message.data.room_id
    robot.logger.debug room_id
    msg.send room_id

  robot.router.post "/hubot/circle", (req, res) ->
    robot.send_room "finished build"
```

`/hubot/circle` を CircleCI から叩かせるつもり。
`req` に CircleCI からの情報がはいってくるので、ちょっと料理する予定。

### 参考文献

* [idobata/hubot-idobata · GitHub](https://github.com/idobata/hubot-idobata)
* [hubot/docs/deploying/heroku.md at master · github/hubot · GitHub](https://github.com/github/hubot/blob/master/docs/deploying/heroku.md)
