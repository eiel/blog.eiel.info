---

title: "nginxの最新版を Debian squeezeで"
date: 2012-07-11T14:19:00+09:00
comments: true
tags: [nginx,debian,apt]
---
nginxはいままでソースからビルドしてたんですが、公式でパッケージ配布されてるのでいい加減aptでインストールできるようにしました。特に特殊なこともしてなかったですし。


gpgのキーが必要なので登録します。


```
wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
```

sources.listにsourceを追加します。

``` bash /etc/apt/sources.list.d/nginx.list
deb http://nginx.org/packages/debian/ squeeze nginx
deb-src http://nginx.org/packages/debian/ squeeze nginx
```

`/etc/apt/sources.list.d/`で設定する場合ファイル名の接尾語が.listである必要があります。


あとはupdateしてsafe-upgrade
```
$ sudo aptitude update
$ sudo aptitude safe-upgrade
```

/etc/nginx/conf.d/に設定を移動して

```
$ sudo /etc/init.d/nginx configtest
```

でOKがでれば

```
$ sudo /etc/init.d/nginx start
```
