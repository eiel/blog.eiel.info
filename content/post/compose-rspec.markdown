---

title: "「私はRSpecでテストをこんな感じで書いてる」に少し便乗してみる"
date: 2012-08-21T19:51:00+09:00
comments: true
tags: [ruby,rspec]
---

[私はRSpecでテストをこんな感じで書いてる](http://d.hatena.ne.jp/sinsoku/20120820/1345470914)という良エントリがあったので少し便乗してみます。

まずは上記の記事を。

最終的なrspecについてですが、私の場合は以下のような感じにしてます。
といっても、前回もかいたように試行錯誤の毎日です。

```ruby
# -*- coding: utf-8 -*-
require_relative 'user'

describe User do
  describe "#admin?" do
    subject { user.admin? }
    let(:user) { User.new(role: role) }

    context "管理者の場合" do
      let(:role) { 'admin' }

      it { should be_true }
    end

    context "一般ユーザの場合" do
      let(:role) { nil }

      it { should_not be_true }
    end
  end

  describe "#runnable_system?" do
    subject { user.runnable_system? }
    let(:user) { User.new(name: name) }

    context "管理者がリンディさんの場合" do
      let(:name) { 'Lindi' }

      before do
        user.stub!(admin?: true)
      end

      it { should be_true }
    end
  end
end
```

diffもつけておきます。

```
@@ -3,30 +3,34 @@
 
 describe User do
   describe "#admin?" do
+    subject { user.admin? }
+    let(:user) { User.new(role: role) }
+
     context "管理者の場合" do
-      before { @admin_user = User.new(role: 'admin') }
+      let(:role) { 'admin' }
 
-      subject { @admin_user }
-      it { should be_admin }
+      it { should be_true }
     end
 
     context "一般ユーザの場合" do
-      before { @user = User.new(role: nil) }
+      let(:role) { nil }
 
-      subject { @user }
-      it { should_not be_admin }
+      it { should_not be_true }
     end
   end
 
   describe "#runnable_system?" do
+    subject { user.runnable_system? }
+    let(:user) { User.new(name: name) }
+
     context "管理者がリンディさんの場合" do
+      let(:name) { 'Lindi' }
+
       before do
-        @lindi = User.new(name: 'Lindi')
-        @lindi.stub!(admin?: true)
+        user.stub!(admin?: true)
       end
 
-      subject { @lindi }
-      it { should be_runnable_system }
+      it { should be_true }
     end
   end
 end
```

とりあえず、便乗しやすいように 用意したuser.rbもつけておきます
```ruby
class User
  def initialize(attributes)
    @attributes = attributes
  end

  def admin?
    @attributes[:role] == 'admin'
  end

  def runnable_system?
    self.admin? and @attributes[:name] == 'Lindi'
  end
end
```

rspecの実行結果。

```
User
  #admin?
    管理者の場合
      should be true
    一般ユーザの場合
      should not be true
  #runnable_system?
    管理者がリンディさんの場合
      should be true
```

違いとしては

* `subject` はできるだけ `describe 'メソッド名'` の直後にかく
* context によって変化する部分は `let` で明確にする
* インスタンス変数は使わず`let`でなんとかする

`let`や`subject`はネストが深い位置であれば上書きします。
`let`を使用した場合は呼ばれない場合、処理されないので少し注意が必要です。

## `subject` はできるだけ `describe 'メソッド名'` の直後にかく
`describe 'メソッド名``内のブロックではテストするブロックが基本的に変化しないのでこの位置に極力かきたいです。
また subjectなので上から読んだときに先に明確にしたいという意図です。


## context によって変化する部分は `let` で明確にする

contextの直前だけ切り出してみます。
```
context "管理者の場合" do
  let(:role) { 'admin' }

context "一般ユーザの場合" do
  let(:role) { nil }
```
roleの部分が変化しますよー。ってのがシンプルになります。

## インスタンス変数は使わず`let`でなんとかする

利点は自分でも整理できてません。とりあえず、今はそういう風にしてる程度です。

## 個人的になやんでること

日本語でcontextをかくとき 語尾に `の場合` とか `のとき` とかくことになるのですが、なんかこれがめんどくさいし。英語の場合は大抵先頭に `when` ってかかれているようです。いっそここだけ英語にしようかなーとか悩んでます。

# 個人的に追加したいリンク

[https://www.relishapp.com/rspec](https://www.relishapp.com/rspec)

公式サイトからもリンクがありますが、
ここには cucumberによって生成されるHTMLがあります。
Rdocより説明が詳しい部分がいろいろあります。

# 最近のrspecの日本語の話題すくなくね？

ということでみんないろいろ情報交換したいです。


# ひとりごと

あー、トラックバックとばす、良い方法ないかなー。

<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_blank&bc1=000000&IS2=1&nou=1&bg1=FFFFFF&fc1=000000&lc1=666666&t=eiel-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4798121932" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
