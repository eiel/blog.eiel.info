---

title: "Mac で docker 起動できない時。具体的にはDOCKER_HOSTを指定してない時"
date: 2014-07-10T18:27:00+09:00
comments: true
categories: ["mac","docker"]
---

DOCKER_HOST を指定しとけって話。

個人的には Docker を外部マシンで起動して使えないか検討したいが、ここでは関係ない。

Macだと VirtuaBox 上で docker が動くので DOCKR_HOST を設定してないといけない。

その状態で使おうとすると下記のようになる。

```
http:///var/run/docker.sock/v1.13/containers/create: dial unix /var/run/docker.sock: no such file or directory
```

`boot2docker up` を実行すると設定すべき値を確認できる。

```
$ boot2docker up
Waiting for VM to be started...

Started.
To connect the Docker client to the Docker daemon, please set:
    export DOCKER_HOST=tcp://xx.xx.xx.xx:2375
```

表示された `export DOCKER_HOST=tcp://xx.xx.xx.xx:2375` をコピペしとけばいい。

設定ファイルで export しとけって。
