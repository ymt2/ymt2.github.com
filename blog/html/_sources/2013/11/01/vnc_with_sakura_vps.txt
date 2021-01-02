=======================================================
 [Ubuntu][VNC]さくらの VPS で VNC をはじめるまでのメモ
=======================================================

とにかくインスタントに VNC をはじめるための手順です。


環境
====

- さくらの VPS
- Ubuntu 12.0.4 amd64


手順
====

デスクトップ環境のインストール
------------------------------

.. code-block:: sh

   $ sudo apt-get install --no-install-recommends ubuntu-desktop


VNC サーバのインストール
------------------------

.. code-block:: sh

   $ sudo apt-get install vnc4server


VNC サーバを起動して初期設定
----------------------------

初回起動で、パスワードの設定を行ないます。とりあえず起動したらすぐ停止。同時に `~/.vnc` 以下に設定ファイルが配置されます。

.. code-block:: sh

   $ vncserver :1
   $ vncserver -kill :1


VPS 接続時に GNOME が起動するように下記の設定を追記。

.. code-block:: sh

   $ echo "gnome-session --session=ubuntu-2d &" >> ~/.vnc/xstartup


設定ができたのでもう一回起動。

.. code-block:: sh

   $ vncserver :1


Mac OS X から VNC サーバへ接続
------------------------------

Mac OS X から接続してみましょう。デフォルトで VNC クライアントが入っているなんて Mac さん素敵。


1. Finder で ⌘+k を叩いて「サーバへ接続」を開く
2. 「サーバアドレス」に「vnc://user_name@ip_address:5901」を入力
3. 「接続」で「画面共有」が立ち上がり GNOME デスクトップを利用できるようになります。


.. author:: default
.. categories:: tips
.. tags:: vps,vnc,ubuntu,mac os x
.. comments::
