---

title: "re: Rails update時に値を変更して更新したい"
date: 2014-09-24T23:23:00+09:00
comments: true
categories: ["rails"]
---

友人のブログ記事へのレスです。
[JunkBox～主に個人的防備録～: Rails update時に値を変更して更新したい。](http://akira-junkbox.blogspot.jp/2014/09/rails-update.html)について。

さすがにやってることがまわりくどい。コメント欄だと返信がつらいのでここにかく。

まず、元の内容を引用します。

> たとえば、保存時１０円単位で四捨五入して保存したい場合。
> ついでに登録日を１日ずらす。
>
> 日付型の項目は、年、月、日、時、分、秒と別れてパラメタに入ってくるので、扱いづらい。ので、一旦モデルに突っ込んで処理し、その後ハッシュに変換する。

```ruby
def update
  # パラメタを一旦モデルに突っ込む
  tmp = Syohin.new(syohin_params)
  # 金額を10円単位で丸める
  tmp.kingaku = tmp.kingaku.round(-1)
  # 日付型の登録日を１日ずらす。
  tmp.record_datetime = tmp.record_datetime + (60 * 60 * 24)
  # モデルをハッシュに変換する。
  tmp2 = tmp.attributes
  # 不要なキーを削除
  ["id","created_at","updated_at"].each do |key|
    tmp2.delete(key)
  end
  #updateに突っ込む
  respond_to do |format|
    if @syohin.update(tmp2)
      format.html { redirect_to @syohin, notice: '更新完了' }
    else
      format.html { render :edit }
    end
  end
end
```

わざわざ一時的な新しいモデルをつくっているけど、updateメソッドのsaveしなバージョンのメソッドがある。assign_attributesメソッドである。



```
def update
  @syohin.assign_attributes(syohin_params)
  @syohin.kingaku = @syohin.kingaku.round(-1) # 金額を10円単位で丸める
  @syohin.record_datetime += 1.day # 日付型の登録日を１日ずらす。
  respond_to do |format|
    if @syohin.save
      format.html { redirect_to @syohin, notice: 'Syohin was successfully updated.' }
      format.json { render :show, status: :ok, location: @syohin }
    else
      format.html { render :edit }
      format.json { render json: @syohin.errors, status: :unprocessable_entity }
    end
  end
end
```

だいぶすっきりした。

ついでに日付だけど、1日増加させたいなら 1.day を足すほうが確実によみやすいので変更した。

### そもそもcontrollerに処理を書きたくない

整理したところで、これらの処理はコントローラレイヤーに書くべきだろうか。
モデルで処理しよう。

コントローラは最初の状態にもどそう。なにも変更は必要ない。

コールバックbefore_updateを使う。

```ruby
class Syohin < ActiveRecord::Base
  before_update do
    self.kingaku = kingaku.round(-1)
    self.record_datetime += 1.day
  end
end
```


今回はないけど、新規の時も+1したいし、表示するときは+1してない値にしたいとかになるとまた別のテクニックが必要になりそうだけど今回は必要がないので割愛。


### 動作確認用のコード

タグ v1 と v2 をきってあるので動かしたい場合はそこにチェックアウトしてください。

* [modified-update-rails-sample](https://github.com/eiel/modified-update-rails-sample)
