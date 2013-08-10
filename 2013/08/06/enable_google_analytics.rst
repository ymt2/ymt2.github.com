===========================================
 Tinkerer で Google Analytics を有効化する
===========================================

折角公開したので、 `Google Analytics`_ をのせてみました。

まぁ、Tinkerer の `ドキュメント`_ を読めば書いてあることです。


トラッキングコードを保存して
========================

``_static/google_analytics.js`` にトラッキングコードをコピペ

.. code-block:: sh

   $ cd {Tinkerer のルートディレクトリ}
   $ mkdir _static
   $ touch _static/google_analytics.js

テンプレートを作り
==================

.. code-block:: sh

   $ touch _templates/page.html


テンプレートからトラッキングコードを読み込んで
==============================================

``_templates/page.html`` を編集

.. code-block:: html

   {% extends "!page.html" %}

   {% set script_files = script_files + ["_static/google_analytics.js"] %}


ビルドしておわり
================

.. code-block:: sh

   $ tinker -b


.. author:: default
.. categories:: sphinx
.. tags:: tinkerer,google analytics,sphinx
.. comments::

.. _`Google Analytics`: http://www.google.co.jp/intl/ja/analytics/
.. _`ドキュメント`: http://tinkerer.me/doc/more_tinkering.html
