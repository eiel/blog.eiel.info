---

title: "rbenv install したときに 一緒に bundler をインストールする"
date: 2013-04-12T02:33:00+09:00
comments: true
tags: ["ruby"]
---

`rbenv install` したときに bundler ぐらい自動で入って欲しいですよね。
たぶん。今日、そういうツイートを見ました。

rbenv には hook という機能が用意されているのでこれを利用するとできます。
hook したときに呼ばれるスクリプトは以下の場所に配置できます。

* ${RBENV_ROOT}/rbenv.d
* /usr/local/etc/rbenv.d
* /etc/rbenv.d
* /usr/lib/rbenv/hooks

環境変数 `RBENV_HOOK_PATH` を設定すると好きな場所に配置できます。
私はホームディレクトリにおきたいので指定しました。

```
export RBENV_HOOK_PATH="$HOME/.rbenv.d"
```

必要に応じて `.zshenv` や `.bash_profile` などに書き込みましょう。

以下、 .rbenv.d に設定したと仮定して話をします。

`rbenv install` 時 にhook するスクリプトは `.rbenv.d/install` におきます。
拡張子を `bash` にする必要があります。

`.rbenv.d/install/install_bundler.bash` というファイルを作成して、

```bash
#/bin/sh

after_install 'RBENV_VERSION="$VERSION_NAME" gem install bundler'
```

とかいておけばOKです。

```
$ rbenv hooks install
```

で rbenv install で hook される script を確認することができます。

[rbenv-hooks](https://github.com/eiel/rbenv-hooks)というリポジトリをつくったので、作るのがめんどくさい人は環境変数を設定して、clone してください。

```
$ git clone git://github.com/eiel/rbenv-hooks.git ~/.rbenv.d
```

READMEかかなきゃ…。

### もうちょっと詳しく

`after_install` というのは ruby-buildの `rbenv-install` で定義されてる関数です。
同様の関数として `before_install` があります。
このへんは ruby-buildの独自機能のようです。

rbenv hooks 自体は rbenv の機能です。
rbenv の各コマンド実行後に実行したいスクリプトがあれば、同じ手法が使えます。

ただ、対応してないコマンドもあるようなので、使いたくなったら、適当に追記して pull request すればよいと思います。(たぶん)

```
# Load plugin hooks.
for script in $(rbenv-hooks コマンド名); do
  source "$script"
done
```

とすると、hook を実行することができます。

hook の一覧を取得してひとつづつ source するだけみたいです。

### おまけ hook を書くときの debug の方法

hook をかくときにどのような変数が定義されてるかは set というコマンドを使えばわかります。
ファイルをつくったら set を利用して使いたい値がないか探してみましょう。

### 関連

* [Rbenv で Ruby の Version を一時的に切り替え](http://blog.eiel.info/blog/2013/04/12/rbenv-version-switch/)
