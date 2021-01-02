=========================================================
 DB マイグレーションツール Flyway で fail したときのメモ
=========================================================

最近プロジェクトに DB マイグレーションツールであるところの、 `flyway`_ を導入しました。

SQL ベースで DB の状態を管理していくので、特定の O/R マッパーに依存することもなく、プロジェクトが走り出してからさくっと導入するぶんには特に問題も起こりませんでした。

そんななか `migrate` が `fail` したときに少々困ったのでメモ。


`fail` するとどうなるか
=======================

`fail` するとこんな感じでエラーメッセージが表示されて処理が停止します。

.. code-block:: sh
   :linenos:

   $ ./flyway.sh migrate
   Flyway (Command-line Tool) v.2.2.1

   Current version of schema `HOGEDB`: 3.1
   Migrating schema `HOGEDB` to version 3.2
   ERROR: com.googlecode.flyway.core.api.FlywayException: Migration of schema `HOGEDB` to version 3.5 failed! Please restore backups and roll back database and code!
   以下エラー文

`schema_version` テーブルを見てみると、 `version` が `0` になっているレコードが挿入されていました。この状態ではいくら `migrate` コマンドを実行してもマイグレーションは行なわれません。


`repair` してあげよう
=====================

以下のように `repair` コマンドを実行することで、 `success` が `0` なレコードが取り除かれ、 `migrate` コマンドを実行できるようになります。もちろん `migrate` コマンドを実行する前に DB や SQL に起こっている問題は解消しておきましょう。

.. code-block:: sh
   :linenos:

   $ ./flyway.sh repair
   Flyway (Command-line Tool) v.2.2.1

   Metadata table `HOGEDB`.`schema_version` successfully repaired (execution time 00:00.001s).
   Manual cleanup of the remaining effects the failed migration may still be required.


`migrate` できた。

.. code-block:: sh
   :linenos:

   $ ./flyway.sh migrate
   Flyway (Command-line Tool) v.2.2.1

   Current version of schema `HOGEDB`: 3.4
   Migrating schema `HOGEDB` to version 3.5
   Successfully applied 1 migration to schema `HOGEDB` (execution time 00:00.102s).


.. author:: default
.. categories:: tips
.. tags:: flyway,migration,sql
.. comments::

.. _`flyway` : http://flywaydb.org/
