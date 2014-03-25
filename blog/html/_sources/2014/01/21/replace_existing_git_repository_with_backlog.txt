既存のリポジトリを backlog に移行する方法
=========================================

# まず既存リポジトリを移行
$ git remote -v
$ cd ..
$ git clone --mirror ssh://ohaco@54.249.93.113:/home/ohaco/.karaoke.git karaoke_bare
$ cd karaoke_bare
$ git push --mirror zawattinc@zawattinc.git.backlog.jp:/SW/server.git

# ローカルリポジトリの origin を変更
$ cd karaoke
$ git remote set-url origin zawattinc@zawattinc.git.backlog.jp:/SW/server.git

# テストリポジトリの origin を変更
$ git remote set-url origin zawattinc@zawattinc.git.backlog.jp:/SW/server.git


リポジトリを backlog と同期する
-------------------------------

.. code-block:: shell

   * * * * * for i in `seq 0 10 59`;do (sleep ${i} ; ~/bin/src/git_pull.sh) & done; >> ~/logs/git_pull_$(date '+\%Y\%m').log 2>&1


チームのみんなにやってもらうこと
--------------------------------

.. code-block:: shell

   $ git remote set-url origin {your backlog repository url}


.. author:: default
.. categories:: tips
.. tags:: git,backlog
.. comments::
