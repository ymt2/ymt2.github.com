======================================
 Docker 上の MySQL に dump を適用する
======================================

`hub.docker.com OFFICIAL REPOSITORY mysql`_ なんかを起動して dump ファイルを適用したいときは下記でいける。ポイントは `-t` オプションをつけないこと。

```
$ docker exec -i {CONTAINER ID} mysql {CREDENTIALS} {DB} < dump.sql
```


.. author:: default
.. categories:: tips
.. tags:: MySQL,Docker
.. comments::

.. _`hub.docker.com OFFICIAL REPOSITORY mysql`: https://hub.docker.com/_/mysql/
