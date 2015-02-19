---

title: "Railsでコントローラのスペックを試行錯誤中"
date: 2012-08-09T23:42:00+09:00
comments: true
tags: [rails,rspec]
---

Rspec書いてますか？最近なかなか荒れ気味ですが、僕はなんだかんだで嫌いじゃないです。
コントローラのテストは何をすべきかなかなか難しいです。

こんな感じでどうかなーというのを一応紹介しておきます。

# 何をテストするか

基本的には rspec を走られせるとこのコントローラが何をするのかわかるようにすることです。

* どのアクションがどのHTTPメソッドを受けるのか
* どんな変数をビューに渡すのか
* リダイレクトするのか、しないのか
* どんなflashが設定されるのか
* 前提とする状況はなにか

といったあたりがわかるようにしています。

## どのアクションがどのHTTPメソッドを受けるのか

これは単純にdescribeにかくだけですが、コード上ではlet式を利用して request という変数にバインドしてちょっとだけ目立つようにしています。

## どんな変数をビューに渡すのか

ビューに渡す値はインスタンス変数に入れますが、どの変数にどんな型の値が入るのかテストしています。
ビューを先にかくことが多いのでその際にpendingにして追加していくとコントローラかくときにビューの確認をする必要がありません。
あと、before_filterのようなものを利用しているの、その中で勝手にバインドするものがあるのでこれを明確にしてやったりします。

## リダイレクトするのか、しないのか

対応するビューがあるのかないのか、うまくいくのどこの画面にいくのかが明確になるのでかいておきます。

## どんなflashが設定されるのか

これは変数の場合とだいたい一緒です。ビューではなく cucumber でのテストとの橋渡しな感じもあります。私はcucumberでは成功したらこの値が出てるのか確認してます。

## 前提とする状況はなにか

ログインしている場合なのか、とかです。contextのブロックが増えるだけです。


# それらを踏まえて上での雛形

コメントは解説のために。

```ruby
describe HogeController do
  subject { request }

  describe "GET 'index'" do
    let(:request) { get :index, params }
    let(:params) { { hoge: "mogumogu"} }

    context "ログインしている時" do
      include_context "ログイン"

      # リダイレクトなどしない場合
      it { should be_success }

      context "リクエストした時" do
        # これをやらないとこの先のブロックが1行な it でかけない
        before(:each) { request }

        # ビューへ渡す変数のの確認
        describe "@hoge" do
          subject { assigns :hoge }
          it { should be_kind_of(Hoge) }
        end

        # flashの確認
        describe "flash[:notice]" do
          subject { flash[:notice] }
          it { should eq("hoge") }
        end
      end
    end

    context "ログインしていない時" do
      # リダイレクトする場合
      it { should redirecto_to("/hoge") }
    end
  end
end
```

といった感じに書いてます。
```ruby
describe "@hoge" do
 subject { assigns :hoge }
end
```
と、かくのはめんどくさいので
```ruby
describe_assigns :hoge do
end
```
と、かけるようにしてたりするのですが、それはまた別の話。

個人的にどうにかしたいのば be_success マッチャー。失敗した場合に falseとでちゃうだけなので、statusコードなどを出すとデバッグも楽になりそうです。

あとは be_kind_of も結構微妙な出力をするので悩み中。リストの時もなんとかしたいです。

あとは単体テストのようで、対応するビューがなかったりすると動かなかったりするのはなんとかしたいです。(なんとかなってた気がするんだけどなぁ。)

Railsのテストについて話し合う人が周りにいないので、誰かお話ししましょう。ヘルプミー。
