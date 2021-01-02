================================================
 [MySQL][マーケティング] \*n 層の分類をする SQL
================================================

性別、生年月日(8桁数値)のカラムを持つテーブルのデータをマーケティングでよくみる `([MF][1-3]|C|T)` 層に分類したところ、えらい残念な SQL ができたのでメモ。

Fn層の分類についてはこの辺 ( `F1層 とは - コトバンク`_ ) を参照のこと。

テーブルはこんな感じでした。

.. code-block:: sql
   :linenos:

   CREATE TABLE `user` (
      `gender` TINYINT(1) NOT NULL, -- 0が男性, 1が女性
      `birthday` CHAR(8) NOT NULL,
   );


で、分類する SQL は以下。もうちょいマシにならないものか。今回、子供を正確に分類する必要がなかったため [MF]0 層として計上しています。

.. code-block:: sql
   :linenos:

   SELECT b.class, COUNT(b.class) AS count
   FROM (
     SELECT CONCAT(
       CASE gender WHEN 0 THEN 'M' ELSE 'F' END,
       CASE
         WHEN a.age < 20 THEN '0'
         WHEN a.age < 25  THEN '1'
         WHEN a.age < 35 THEN '2'
         ELSE '3' END
       ) AS class
     FROM (
       SELECT
         (YEAR(CURDATE()) - YEAR(date_format(birthday,'%Y-%m-%d')))
         - (RIGHT(CURDATE(),5) < RIGHT(date_format(birthday,'%Y-%m-%d'),5)) AS age
       FROM user
     ) a
   ) b
   GROUP BY b.class


こんな感じの結果が返ってきます。

.. code-block:: sh

   +-------+-------+
   | class | count |
   +-------+-------+
   | F0    |   100 |
   | F1    |   200 |
   | F2    |   300 |
   | F3    |   400 |
   | M0    |   100 |
   | M1    |   200 |
   | M2    |   300 |
   | M3    |   400 |
   +-------+-------+


いちおう分類表も書いておきます(表組みの練習もかねて)。

.. csv-table:: ターゲットユーザの分類
   :header-rows: 1

   表記, 性別, 年齢層
   M1, 男性, 20 - 34 歳
   M2, 男性, 35 - 49 歳
   M3, 男性, 50 歳以上
   F1, 女性, 20 - 34 歳
   F2, 女性, 35 - 49 歳
   F3, 女性, 50 歳以上
   C, 男女, 4 - 12 歳
   T, 男女, 13 - 19 歳


.. author:: default
.. categories:: tips
.. tags:: marketing,mysql,sql
.. comments::

.. _`F1層 とは - コトバンク` : http://kotobank.jp/word/F1%E5%B1%A4
