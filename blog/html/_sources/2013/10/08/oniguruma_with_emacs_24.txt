==================================================================
[Emacs 24] ふつうの正規表現を使いたい foreign-regexp で 鬼車を使う
==================================================================


`Emacs の正規表現`_ はほんとにつらい。 `syntax_classes` のブラケットにもバックスラッシュが必要だったりととにかくバックスラッシュ地獄で、リプレイスするのにわざわざ別のエディタ使うレベル。

`foreign-regexp`_ でラクしましょう。外部コマンドに `Ruby` を指定することで、 `豊富な表現で再帰まで使える`_ 正規表現ライブラリ `鬼車`_ をインクリメンタルサーチやリプレイスに利用できます。


インストールした環境
====================

- Mac OS X 10.8.5
- Emacs 24.3.1
- ruby 1.9.3p392 (rbenv)


インストール手順
================

`package-install` できます。これを `*scratch*` バッファにでもコピーして `C-x C-e` でおしまい。

.. code-block:: scheme

   (package-install 'foreign-regexp)


`.emacs` に追記。 `foreign-regexp/regexp-type` には `perl` / `ruby` / `javascript` が指定できるようです。今回は `鬼車` が使いたいので `ruby` を指定。

.. code-block:: scheme

   (cond ((require 'foreign-regexp nil t)
          (custom-set-variables
	  '(foreign-regexp/regexp-type 'ruby)
	  '(reb-re-syntax 'foreign-regexp))))


使い方をさっくりと
==================

インクリメンタルサーチ
  M-s M-s

インクリメンタルサーチ(逆方向)
  M-s M-r

リプレイス
  M-s M-%


これでわかりやすい正規表現を使える!!
------------------------------------

エスケープ地獄から開放される幸せ。

例：連続したひらがなをインクリメンタルサーチ
  M-s M-s \\p{Hiragana}+

例：連続したひらがな以外の文字をインクリメンタルサーチ
  M-s M-s \\p{^Hiragana}+

がんばるともっといろいろできます。
`回文や XML にマッチする鬼車の正規表現 - まめめも`_


.. _`Emacs の正規表現` : http://www.emacswiki.org/emacs/RegularExpression
.. _`foreign-regexp` : https://github.com/k-talo/foreign-regexp.el
.. _`豊富な表現で再帰まで使える` : http://d.hatena.ne.jp/ku-ma-me/20090409/p1
.. _`鬼車` : http://www.geocities.jp/kosako3/oniguruma/index_ja.html
.. _`回文や XML にマッチする鬼車の正規表現 - まめめも` : http://d.hatena.ne.jp/ku-ma-me/20090409/p1

.. author:: default
.. categories:: tips
.. tags:: emacs24,regexp,oniguruma,ruby
.. comments::
