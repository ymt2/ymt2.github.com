=====================================
 Tinkerer でブログをはじめてみました
=====================================


Sphinx_ を使ってブログが書ける Tinkerer_ を発見したのではじめてみました。
ホスティングは GitHub_ を利用しています。


GitHub でブログを公開するまでにやったこと
=========================================

virtualenv で仮想環境を作る
---------------------------

Sphinx の拡張機能をいろいろ試したかったので仮想環境を用意しました。Github Pages に公開したかったので Git リポジトリも同時に作成。


.. code-block:: sh

   $ cd {your_git_directory}
   $ git init hoge
   $ cd hoge
   $ mkvirtualenv hoge


あと、 ``$ workon hoge`` とかやってすぐに書きはじめられるといいなーと思ったので ``$VIRTUAL_ENV/bin/.postactivate`` を以下のように編集。これで ``workon`` したときに Git リポジトリのあるディレクトリに移動してくれます。


.. code-block:: sh

   #!/opt/local/bin/zsh
   # This hook is run after this virtualenv is activated.

   proj_name=$(echo $VIRTUAL_ENV|awk -F'/' '{print $NF}')
   cd {your_git_directory}/hoge


Tinkerer のインストールから記事を書くまで
-----------------------------------------

Tinkerer インストール
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: sh

   $ pip install tinkerer

ブログを作る
~~~~~~~~~~~~

.. code-block:: sh

   $ tinker -s


カレントディレクトリに Sphinx のプロジェクトが展開されます。まずは ``conf.py`` をいじってブログタイトルなどを設定しましょう。 ``TODO:`` から始まるセクションを編集します。

記事を書く
~~~~~~~~~~

以下のコマンドで rst ファイルが生成されるので編集しましょう。 ``blog`` ディレクトリ内に rst ファイルが配置されます。

.. code-block:: sh

   $ tinker -p title

ちなみに、以下のコマンドで単一のページを作成することも可能です。こちらは ``pages`` ディレクトリ内に rst ファイルが配置されます。

.. code-block:: sh

   $ tinker --page title

記事が書けたら、 ``$ tinker -b`` とするとビルドが実行されます。 ``index.html`` が生成されるのでブラウザで開いて出来ばえを確認できます。


GitHub Pages で公開
-------------------

`Hosting on GitHub`_ とか `GitHub Help`_ を参考に設定。注意点としては、リポジトリのルートに ``.nojekyll`` ファイルを作っておくことと、ブログのデータをコミットするブランチを ``gh-pages`` としておくことが必要です。


思ったこと
==========

Sphinx で書けるので、当然 Blockdiag_ なんかも使えます。
うまいことテーマをいじって wiki とブログを組み合わせたような使い方ができないか考え中。


.. _Sphinx: http://sphinx-doc.org/
.. _Tinkerer: http://tinkerer.me/
.. _GitHub: https://github.com/
.. _Blockdiag: http://blockdiag.com/ja/blockdiag/
.. _`Hosting on GitHub`: http://tinkerer.me/doc/deploying.html#hosting-on-github
.. _`GitHub Help`: https://help.github.com/categories/20/articles

.. author:: default
.. categories:: sphinx
.. tags:: sphinx,python,tinkerer
.. comments::
