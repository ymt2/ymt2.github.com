+++
title = "code-server"
author = ["ymt2"]
date = 2022-01-09T23:29:00+09:00
lastmod = 2022-01-10T16:02:37+09:00
tags = ["linux", "arch-linux", "code-server"]
categories = ["environment"]
draft = false
weight = 2003
+++

最近はブラウザでコードを書くのが流行っていそうなので、自宅マシン上の
[coder/code-server](https://github.com/coder/code-server) をどこにいても利用できるようにしてみた。

所有している端末のどれでも、どこからでも接続したいが安心は欲しいので
VPN 経由でのみ接続できるようにしている。

VPN の構成には [Tailscale](https://tailscale.com/) を利用している。
自前で構築してもよかったが、住んでいるマンションのインフラ都合上、手元の
ルータにグローバル IP アドレスが降ってこないので面倒くさそうでやめた。

以下のような構成を目指す。
![](https://user-images.githubusercontent.com/964112/148686035-6b87f23b-28f9-43bc-b7a1-6cdb5c9c2047.png)


## code-server の構成 {#code-server-の構成}

インストール自体は簡単。

```shell
$ paru -S code-server
$ systemctl enable code-server@$USER
```

大事なことはリバースプロキシに任せるので設定も適当。

<a id="code-snippet--~-.config-code-server-config.yaml"></a>
```nil
bind-addr: 127.0.0.1:8080
auth: none
cert: false
```


## Tailscale の構成 {#tailscale-の構成}

code-server は非 SSL アクセスな場合にその機能を制限してしまう。

Tailscale には内部のアクセスのための SSL 証明書を発行する仕組みが
備わっているので、それを活用して code-server の制限を回避する。
<https://tailscale.com/blog/tls-certs/>

手順は [Enabling HTTPS - Tailscale](https://tailscale.com/blog/tls-certs/) にある。
完了すると /var/lib/tailscale/certs あたりに必要なファイルが揃っているはず。


## nginx の構成 {#nginx-の構成}

```shell
$ paru -S nginx-mainline
$ systemctl enable nginx.service
```

[virtual servers を管理しやすくする設定](https://nginx.org/en/docs/http/request_processing.html)。

<a id="code-snippet---etc-nginx-nginx.conf"></a>
```nil
http {
    ...
    server {
        ...
        include sites-enabled/*;
    }
}
```

ところで、[Tailscale にて割り振られる IP アドレスの範囲は決まっている](https://tailscale.com/kb/1015/100.x-addresses/)ので、
範囲外からのアクセスは nginx に接続を切ってもらうようにする。

<a id="code-snippet---etc-nginx-nginx.conf"></a>
```nil
http {
    ...
    geo $geo_allowed {
        default 0;
        100.64.0.0/10 1;
    }
}
```

Tailscale で生成した SSL 証明書を喰わせる設定をする。

<a id="code-snippet---etc-nginx-sites-available-code-server.conf"></a>
```nil
server {
    listen 80;
    listen [::]:80;
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name TAILSCALE_MACHINE_NAME.TAILSCALE_DOMAIN.SUFFIX;

    ssl_certificate /var/lib/tailscale/certs/CERT_NAME.crt;
    ssl_certificate_key /var/lib/tailscale/certs/KEY_NAME.key;

    if ($geo_allowed = 0){
        return 444;
    }

    location / {
        proxy_pass http://localhost:8080/;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection upgrade;
        proxy_set_header Accept-Encoding gzip;
    }
}
```

/etc/nginx/sites-available/code-server.conf から
/etc/nginx/sites-enabled/code-server.conf にシンボリックリンクを張る。

以上でブラウザから code-server を利用することができるようになっているはず。
