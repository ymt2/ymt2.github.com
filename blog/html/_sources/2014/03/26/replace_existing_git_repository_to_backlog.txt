============================================
 既存の git リポジトリを backlog に移行する
============================================

自前で管理していた共有の Git リモートリポジトリを `backlog`_ に移行したときのメモです。


リポジトリの移行
================

リモートリポジトリをローカルに clone している前提です。

.. code-block:: sh

   $ cd {ローカルリポジトリのルート}
   $ git remote -v # リモートリポジトリの URL を確認
   $ cd ..
   $ clone --mirror {リモートリポジトリ} repository_bare
   $ cd repository_bare
   $ git push --mirror {backlog のリモートリポジトリ URL}


チームへの共有
==============

チームには下記コマンドをローカルリポジトリのルートで叩いてもらいましょう。

.. code-block:: sh

   $ git remote set-url origin {backlog のリモートリポジトリ URL}


.. _`backlog`: http://www.backlog.jp/

.. author:: default
.. categories:: tips
.. tags:: git, backlog
.. comments::
