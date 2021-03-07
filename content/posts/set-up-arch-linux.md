+++
title = "Set up Arch Linux"
author = ["ymt2"]
date = 2021-03-08T00:18:00+09:00
lastmod = 2021-03-08T00:24:04+09:00
tags = ["arch-linux", "linux"]
categories = ["environment"]
draft = false
weight = 2002
+++

お仕事用メイン機として久しぶりに[Arch Linux](https://wiki.archlinux.org/index.php/installation%5Fguide) をセットアップしたので記録しておく。
なお、常に最新の情報源として[Installation guide - ArchWiki](https://wiki.archlinux.org/index.php/installation%5Fguide)を参照するべきことが
前提。


## 構成 {#構成}

| Board   | ASRock X570 Phantom Gaming 4                          |
|---------|-------------------------------------------------------|
| CPU     | AMD Ryzen 7 3700X                                     |
| VGA     | ZOTAC GAMING GeForce RTX 2070 SUPER AMP Extreme       |
| RAM     | DDR4-3200 16GB x 2                                    |
| Storage | nvme0: Windows, nvme1: TARGET, ssd0: Data for Windows |


## 事前準備 {#事前準備}


### インストールメディアの作成 {#インストールメディアの作成}

セットアップ対象のマシンは既に Windows マシンとして構成していたため [Rufus](https://rufus.ie/)
を利用して USB インストールメディアを作成した。

[USB flash installation medium - ArchWiki](https://wiki.archlinux.org/index.php/USB%5Fflash%5Finstallation%5Fmedium#Using%5FRufus) には以下のように書かれているが、こ
のオプションは「スタート」を押して実際に書き込む直前でないと表示されないこ
とに戸惑った。

> Note: If the USB drive does not boot properly using the default ISO Image mode, DD Image mode should be used instead. To switch this mode on, select GPT from the Partition scheme drop-down menu. After clicking START you will get the mode selection dialog, select DD Image mode.


## セットアップ {#セットアップ}

ひたすら[Installation guide - ArchWiki](https://wiki.archlinux.org/index.php/installation%5Fguide)を参照してやる。

設定はできるだけ [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) に沿っているほうが
git 管理しやすいので[XDG Base Directory - ArchWiki](https://wiki.archlinux.org/index.php/XDG%5FBase%5FDirectory) を横目に諸々行なった。


## パッケージマネージャ {#パッケージマネージャ}

AUR helper [paru](https://github.com/morganamilo/paru) を利用することにした。


## デスクトップ環境 {#デスクトップ環境}


### Display manager {#display-manager}

[LightDM](https://github.com/canonical/lightdm) を利用することにした。
[LightDM - ArchWiki](https://wiki.archlinux.org/index.php/LightDM) を参照すればセットアップに迷うことはなさそう。

ログイン画面には [lightdm-webkit2-greeter](https://archlinux.org/packages/?name=lightdm-webkit2-greeter) を採用。


### Window manager {#window-manager}

[xmonad](https://xmonad.org/) を使用することにした。
[xmonad.hs](https://github.com/ymt2/dots/blob/master/dot.config/xmonad/xmonad.hs) は git 管理している。

KDE や GNOME のようなデスクトップ環境と比較して xmonad はウィンドウマネージャ
としての機能に特化したもので機能の不足を感じたため他の手段で補填することに
した。

具体的には以下のような [Xfce](https://www.xfce.org/) のモジュールを部分的に採用した。
インストールは `$ paru -S xfce4-goodies` でやった気がする。


#### デスクトップ通知 {#デスクトップ通知}

チャットツールなどによる通知は仕事でのやりとり上あったほうがよいので [Xfce
Notification Daemon](https://docs.xfce.org/apps/notifyd/start) を D-Bus 経由で動作させることにした。

`~/.local/share/dbus-1/services/org.freedesktop.Notifications.service` に
以下のような設定を書く程度で動作する。

```nil
[D-BUS Service]
Name=org.freedesktop.Notifications
Exec=/usr/lib/xfce4/notifyd/xfce4-notifyd
```

動作チェックには `$ notifyd-send MESSAGE` とかやればよい。


### スクリーンショット {#スクリーンショット}

xfce4-screenshooter を `<Print>` (フルスクリーン) or `C-<Print>` (範囲選択モード) で起動するようにした。

[xfce4-clipman](https://docs.xfce.org/panel-plugins/xfce4-clipman-plugin) をバックグラウンドで動作させておくとキャプチャしたものを
直接クリップボードに載せて、そのままブラウザにペーストして GitHub Issue に
貼るなどができ便利。


### IME {#ime}

TBU `ibus-skk`


## サウンド {#サウンド}

alsa-utils と pulseaudio-alsa を入れている。
基本的に alsamixer で事足りている。


## ターミナルエミュレータ {#ターミナルエミュレータ}

TBU `urxvt`


## ビデオ会議 {#ビデオ会議}

仕事のやりとりで Google Meet などビデオ会議ツールを多用するため動作するよう
設定する必要があった。

Zoom, Slack, discord のデスクトップクライアントやブラウザ上で Google Meet,
Whereby の動作を確認済み。


### カメラ周辺の設定 {#カメラ周辺の設定}

[Logicool C922n Pro Stream Webcam](https://www.logicool.co.jp/ja-jp/product/c922n-pro-stream-webcam) は USB 接続するだけで動作した。
[OBS Studio](https://obsproject.com/) を間にはさんでビデオ会議ソフトウェアに接続している。


### 仮想カメラ設定 {#仮想カメラ設定}

OBS Studio の `Start Virtual Camera` ボタンを有効にするため設定が必要。

```shell
$ paru -S v4l2loopback-dkms
$ modprobe v4l2loopback-dkms # 動作チェック。事前に再起動が必要だった気がする
```

動作チェック時に以下のようなメッセージが出るようなら
`$ paru -S linux-headers` する。

> modprobe: FATAL: Module v4l2loopback-dkms not found in directory /lib/modules/5.10.15-arch1-1

動作チェックできたら設定を永続化するために以下をやる。

```shell
$ echo v4l2loopback | sudo tee -a /etc/modules-load.d/v4l2loopback.conf
$ echo 'options v4l2loopback exclusive_caps=1' | sudo tee -a /etc/modprobe.d/v4l2loopback.conf
```


### スクリーンキャプチャ {#スクリーンキャプチャ}

OBS Studio の `Window Capture (Xcomposite)` を動作させるために以下のような
設定を行なった。

-   `Xmonad.Hooks.EwmhDesktops` を xmonad.hs でインポート
-   xmonad オブジェクト初期化時に `ewmh defaultConfig` をはさむ


## パスワードマネージャ {#パスワードマネージャ}

TBU `keepassxc`
