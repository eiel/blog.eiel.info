---

title: "Route 53 の特定の Hosted Zone を IAM ユーザで設定できるようにしてみた"
date: 2014-02-14T15:55:00+09:00
comments: true
categories: ["route53","aws","iam"]
---

AWSの無料枠のうちにいろいろ遊ぼうと思っていたのに半年ぐらいなにもできてないユーザがこちらになります。こんにちは。

そんなわけでAWS入門者です。がんばります。

AWSへの入門するなら [Route 53](http://aws.amazon.com/jp/route53/) や [S3](http://aws.amazon.com/jp/s3/) あたりが良いと思います。EC2 とか SNS とかなかなかハードル高いです。あれ、そういえば9月ぐらいに試してる形跡が…。

先日、eiel.info の管理を Route 53 に移してみたら、気持ち表示が速くなった気がします。たぶん、気のせいです。

他にも代わりに管理してる zone があったので、移行してみました。
いままでレコードを追加するのは私がしてたのですが、「本人にも自由に編集できるといいなぁ」と試してみたらできました。[IAM](http://aws.amazon.com/jp/iam/) を使いました。まー、お金を払うのは自分ですが。+0.5$ 程度なので大したことではないです。


管理コンソールで操作してもらうことを前提にした場合のやることは、

* 管理する Hosted Zone を作成する(Route 53)
* 操作者のためのユーザを作成する(IAM)
* ユーザに作成した Hosted Zone への編集をできるようにする。(IAM ポリシー設定)
* ログインページとログイン情報を伝える
* パスワードを変更して貰う
* 編集ページのURLを伝える

といった感じのようです。

### 管理する Hosted Zone を作成する(Route 53)

管理する Hosted Zone がないとはじまらないので、作成します。
Route 53 の管理コンソールで行えます。

脱線しますが、Bind の zone ファイルからそのままインポートできるようになっていて、移行はとても簡単でした。

作成した Zone  **`Hosted Zone ID`** を控えておきましょう。後で使います。

### 操作者のためのユーザを作成する(IAM)

ログインしてもらうためのユーザが必要です。
IAMで作りましょう。

管理コンソールへのログインはパスワードが必要になります。
デフォルトでは設定されてないので、パスワードを設定したいユーザを選択して、Security Credentials で Password の設定をしましょう。

パスワードはあとで使用者に変更してもらえばよいです。
忘れたら作りなしましょう。

### ユーザに作成した Hosted Zone への編集をできるようにする。

作成したユーザは基本的には何もできない状態です。
特定の zone だけ編集できるようにしてみます。

zone の編集をする行動を許可する作業をします。

IAM でユーザを選択して `permission` > `Attach User Policy` で設定できます。
JSONで指定できるのですが、ジェネレータがあるとでポチポチすれば作成できます。

Policy Generator を選択して Select をするとフォームがでてきます。

* Effect - Allow
* AWS Service - Route 53
* Actions - All Actions
* Amazon Resource Name (ARN) - 後述

と指定しました。

ARN には Hosted Zone Id を使用して

* `arn:aws:route53:::hostedzone/[Hosted Zone ID]`

となります。

このへんの情報は [Using IAM to Control Access to Amazon Route 53 Resources - Amazon Route 53](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/UsingWithIAM.html) に記載されてます。Policy のサンプル JSON なんかものっています。


### ログインページとログイン情報を伝える

準備ができたら相手にユーザ情報を伝えましょう。
ログインするための URL を伝える必要もあります。

ログインするための URL は IAM の Dashboard に IAM User sign-in URL が記載されてます。

* `https://[Account ID].signin.aws.amazon.com/console`

少々長く覚えにくいAccount ID が使用されています。
Alias を作成すると短いものを利用できます。

### パスワードを変更して貰う

ログインしてもらったらパスワードを変えてもらいましょう。
暗号化された通信路を使用したのであれば気にしなくていいかもしれません。

### 編集ページのURLを伝える

早速、ログインして Route 53 のページにいっても何も表示されません。
**Hosted Zone の一覧を取得する権限がないからです。**
直接アクセスすれば表示できるので気にしなくてもいいでしょう。

Hosted Zone ID を使用して

* `https://console.aws.amazon.com/route53/home#resource-record-sets:[Hosted Zone ID]`

にアクセスすれば編集できました。

### その他雑多なこと

Hosted Zone の一覧が表示されて構わないのであれば、 ListHostedZones Action に Resource `*` でポリシーを追加してやれば表示できます。

ListHostedZones には個別のリソースを許可することで表示内容を制限する機能はないようです。

Zoneの設定するのに便利そうな [Roadworker](https://bitbucket.org/winebarrel/roadworker) などなど使う場合も設定しなきゃいけないんだろうか?
パッと見た感じでは Hosted Zone ID を指定する部分がないので入るのでしょうか。

必要な部分だけ編集権限を他人に委譲できるので IAM の使いどころはそこそこありそうですね。
AWS はAPI経由の操作が前提設計な感じになってて最終的には自動化もさせやすくて楽しいですね。

次は [SQS](http://aws.amazon.com/jp/sqs/) を試してみようかと思っていたりします。
