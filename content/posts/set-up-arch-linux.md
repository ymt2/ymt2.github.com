+++
title = "Set up Arch Linux :Arch Linux:"
author = ["ymt2"]
lastmod = 2021-02-22T01:39:16+09:00
draft = true
weight = 2002
+++

お仕事用メイン機として久しぶりに[Arch Linux](https://wiki.archlinux.org/index.php/installation%5Fguide) をセットアップしたので記録しておく。
なお、常に最新の情報源として[Installation guide - ArchWiki](https://wiki.archlinux.org/index.php/installation%5Fguide)を参照するべきことが
前提。


## 構成 {#構成}

| Board | ASRock X570 Phantom Gaming 4                    |
|-------|-------------------------------------------------|
| CPU   | AMD Ryzen 7 3700X                               |
| VGA   | ZOTAC GAMING GeForce RTX 2070 SUPER AMP Extreme |
| RAM   | DDR4-3200 16GB x 2                              |


## 事前準備 {#事前準備}


### インストールメディアの作成 {#インストールメディアの作成}

セットアップ対象のマシンは既に Windows マシンとして構成していたため [Rufus](https://rufus.ie/)
を利用して USB インストールメディアを作成した。

[USB flash installation medium - ArchWiki](https://wiki.archlinux.org/index.php/USB%5Fflash%5Finstallation%5Fmedium#Using%5FRufus) には以下のように書かれているが、こ
のオプションは「スタート」を押して実際に書き込む直前でないと表示されないこ
とに戸惑った。

> Note: If the USB drive does not boot properly using the default ISO Image mode, DD Image mode should be used instead. To switch this mode on, select GPT from the Partition scheme drop-down menu. After clicking START you will get the mode selection dialog, select DD Image mode.


### セットアップ {#セットアップ}

TBU
