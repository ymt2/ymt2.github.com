===============================================================
 migemo を emacs 24.3.1 にインストールしたときのメモ, Mac OS X
===============================================================


migemo とは
===========

`migemo`_ とは、ローマ字を入力することで日本語文章をインクリメンタルサーチするためのソフトウェアです。emacs 拡張を利用すると、いつもの `C-s` がそのまま日本語に対応します。

今回はC言語で実装された `cmigemo` をインストールします。


cmigemo のインストール
======================

github から `cmigemo`_ を入手してインストールします。オプションはデフォルトのままにしておきました。デフォルトでは `/usr/local/` 以下にインストールされます。

.. code-block:: sh

   $ git clone https://github.com/koron/cmigemo.git cmigemo
   $ cd cmigemo
   $ ./configure
   $ make osx
   $ make osx-dict
   $ sudo make osx-install


cmigemo を試してみる
====================

下記のコマンドを実行するとプロンプトが表示され、入力待ちの状態になります。

.. code-block:: sh

   $ cmigemo -d /usr/local/share/migemo/utf-8/migemo-dict
   migemo_open("/usr/local/share/migemo/utf-8/migemo-dict")=0x7fb552c03990
   clock()=0.120343
   QUERY:

`QUERY` 適当なローマ字を入力してみると、正規表現が生成されることがわかります。正規表現を入力の度に生成し、それを検索クエリとすることでインクリメンタルサーチを可能にするわけですね。

.. code-block:: sh

   QUERY: paison
   PATTERN: (ﾊﾟｲｿ[ﾝﾉﾈﾇﾆﾅ]|パイソ[ンノネヌニナ]|ぱいそ[んのねぬにな]|ｐａｉｓｏｎ|paison)
   QUERY: toukyo
   PATTERN: (ﾄｳｷｮ|トウキョ|等距離|登極|当(局|協会)|東[教京]|とうきょ|ｔｏｕｋｙｏ|toukyo)
   QUERY:


migemo.el のインストール
========================

`emacs` 拡張の `migemo.el` は `package.el` を使うと簡単にインストールできます。 `*scratch*` バッファで `(package-install 'migemo)` を評価でもしてやると入ります。


migemo.el の設定
================

`init.el` にこんな感じの設定を追記。 `C-s` してミニバッファに `[MIGEMO] I-search`  と表示されれば日本語インクリメンタルサーチが可能になっているはずです。


.. code-block:: common-lisp

   (require 'migemo)
   (setq migemo-command "/usr/local/bin/cmigemo")
   (setq migemo-options '("-q" "--emacs"))
   (setq migemo-dictionary "/usr/local/share/migemo/utf-8/migemo-dict")
   (setq migemo-user-dictionary nil)
   (setq migemo-coding-system 'utf-8-unix)
   (setq migemo-regex-dictionary nil)
   (load-library "migemo")
   (migemo-init)


.. author:: default
.. categories:: emacs
.. tags:: migemo,emacs,mac os x
.. comments::

.. _`migemo`: http://0xcc.net/migemo/
.. _`cmigemo`: https://github.com/koron/cmigemo
