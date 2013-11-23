========================================================================
MySQL 5.6 を MacPorts で Mac OS X Mavericks にインストールしたときのメモ
========================================================================

`$ sudo port install mysql56-server` でいけると思っていたらどうもビルドがこける。
ぐぐるとパッチを当てて回避しているひとがいるみたいなので試してみた。
http://bugs.mysql.com/bug.php?id=70542


環境とか
========

- Mac OS X 10.9 Mavericks

  - Command line tools インストール済み

- MacPorts 2.2.1
- MySQL 5.6.13


MySQL にパッチを当てる
======================

上記ウェブサイトで `fix-build-on-macosx-9.patch` を検索するとリンクが見つかるので、そこからパッチを落とす。また、以下のコマンドでインストールせずに MySQL のソースだけを落とす。

.. code-block:: sh

   $ cd ~
   $ sudo port clean mysql56-server
   $ sudo port -f patch mysql56-server


ソースのディレクトリに移動してパッチを当てる。

.. code-block:: sh

   $ cd /opt/local/var/macports/build/_opt_local_var_macports_sources_rsync.macports.org_release_tarballs_ports_databases_mysql56/mysql56/work/mysql-5.6.13/include
   $ sudo patch -p 0 < ~/Downloads/fix-build-on-macosx-9.patch


インストールを実行。

.. code-block:: sh

   $ cd ~
   $ sudo port -f install mysql56-server
   $ sudo -u _mysql /opt/local/lib/mysql56/bin/mysql_install_db
   $ sudo port load mysql56-server
   $ /opt/local/lib/mysql56/bin/mysqladmin -u root password 'パスワード'


ログインできたら成功。

.. code-block:: sh

   $ mysql56 -uroot -p


補足
====

`mysqld_safe` が動かなかったので修正した記録と記憶が残っていたのだけど、本当に必要な操作であったかを覚えていない…。念のため diff を貼っておく。

.. code-block:: diff

   --- mysqld_safe.org     2013-11-20 22:35:24.000000000 +0900
   +++ /opt/local/lib/mysql56/bin/mysqld_safe      2013-11-20 22:36:11.000000000 +0900
   @@ -719,24 +719,22 @@
   # Note: The switches to 'ps' may depend on your operating system
   if test -f "$pid_file"
   then
   -  PID=`cat "$pid_file"`
   -  if kill -0 $PID > /dev/null 2> /dev/null
   -  then
   -    #if
   -    #then    # The pid contains a mysqld process
   -    #  log_error "A mysqld process already exists"
   -    #  exit 1
   -    #fi
   -  fi
   -  rm -f "$pid_file"
   -  if test -f "$pid_file"
   -  then
   -    log_error "Fatal error: Can't remove the pid file:
   +    PID=`cat "$pid_file"`
   +    if kill -0 $PID > /dev/null 2> /dev/null
   +    then
   +       # The pid contains a mysqld process
   +       log_error "A mysqld process already exists"
   +       exit 1
   +    fi
   +    rm -f "$pid_file"
   +    if test -f "$pid_file"
   +    then
   +       log_error "Fatal error: Can't remove the pid file:
           $pid_file
           Please remove it manually and start $0 again;
           mysqld daemon not started"
   -    exit 1
   -  fi
   +       exit 1
   +    fi
   fi

   #


.. author:: default
.. categories:: tips
.. tags:: mac os x, mavericks, mysql
.. comments::
