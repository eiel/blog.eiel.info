---

title: "S3を使って静的サイトの公開する奴をしてみた。自動化できるみたいで幸せだった。"
date: 2014-03-04T02:16:00+09:00
comments: true
categories: ["aws","ruby"]
---

AWS 楽しいですね。プログラミングできる領域が増加する楽しさがありますね。

なかなかAWSのマネージメントコンソールとお別れできない、AWS初心者です、こんばんは。

[Amazon S3](http://aws.amazon.com/jp/s3/) の売り文句に静的サイトにするという話がよくあります。
ちょっとやってみたのですが、マネージメントコンソールでのポリシーの設定とかめんどくさい。

具体的にいうと、[S3を使って静的サイトを公開する手順](http://www.slideshare.net/horiyasu/amazon-s3web-27138902/25)に記載されてる1番と2番と3番がめんどくさい。

1. Webサイト用にS3のバケットを設定する。
2. バケット内のファイルがアップロードした際、自動的に公開されるようバケットポリシーを追加する。
3. HTMLファイルをアップロードする。
4. S3のwebsite  endpointにアクセスし、ウェブサイト が表示されることを確認する。

という手順を踏む。

* 1番はデフォルト設定だとそんなにめんどくさくない。
* 2番はコピペして修正しなきゃいけなくて少しめんどくさい。これはやらないと毎回アップしたオブジェクトを公開しないといけない。上書きしたとしても。
* 3番はいろんなツールがありそうな気がする。今回は気にしない。
* 4番はしゃーない。

というわけで、1番と2番を ruby の aws-sdk をつかってやってみた。

```ruby
require 'aws-sdk'

def s3_static_site(bucket_name)
  @hostname = bucket_name
  set_policy
  set_website
end

def set_policy
  bucket.policy = AWS::S3::Policy.from_json(policy_json)
end

def set_website
  bucket.configure_website do |cfg|
    cfg.index_document_suffix = 'index.html'
    cfg.error_document_key = 'error.html'
  end
end

def s3
  region = ENV['AWS_REGION']
  end_point = "s3-#{region}.amazonaws.com"
  @s3 ||= AWS::S3.new(s3_endpoint: end_point)
end

def bucket
  @bucket ||= s3.buckets[@hostname]
end

def policy_json
 <<POLICY_JSON
{
  "Version":"2012-10-17",
  "Statement":[{
        "Sid":"AddPerm",
        "Effect":"Allow",
          "Principal": {
            "AWS": "*"
         },
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::#{@hostname}/*"
      ]
    }
  ]
}
POLICY_JSON
end


s3_static_site ARGV[0]
```

`s3-static-site.rb` とかで保存していると

```
ruby s3-static-site.rb [バケット名]
```

でバケットの設定が終わる。

ただし、事前に環境変数として、`AWS_ACCESS_KEY_ID` `AWS_SECRET_ACCESS_KEY` `AWS_REGION` の設定をしておかないといけません。
`AWS_ACCESS_KEY_ID` `AWS_SECRET_ACCESS_KEY` はIAMでユーザをつくってそこから生成した。
AWS_REGIONは利用したいリージョンで、東京をつかう場合は `ap-northeast-1` を設定する。

いっそバケットの作成とRoute53の設定を組み込んですれば「10秒でS3を使って静的サイト環境を作成」とか言える気がする。
すごく車輪の再発明臭がする。

そういうのなかったら gemとか作りたい。

### まとめ

そんなわけで、AWSはいままでハードウェアだと思ってたものを操作できるということを学んだ。
プログラマーのできることが増えて非常に楽しい。

なんでこういう楽しさをみんなもっとはやく教えてくれないんだ。
(従量課金が怖かったとか言えない)

そんなことを思った。

関係ないけど [Twilio](http://twilio.kddi-web.com/) もプログラミングできるものを増やしてくれて楽しさを感じた。

### 参考文献

* [Amazon S3による静的Webサイトホスティング](http://www.slideshare.net/horiyasu/amazon-s3web-27138902)
* [Class: AWS::S3 — AWS SDK for Ruby](http://docs.aws.amazon.com/AWSRubySDK/latest/AWS/S3.html)
* [Example Cases for Amazon S3 Bucket Policies - Amazon Simple Storage Service](https://docs.aws.amazon.com/AmazonS3/latest/dev/AccessPolicyLanguage_UseCases_s3_a.html)
