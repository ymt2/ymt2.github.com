Supervisor で touch-reload する Eventlistener
======================================================

つくった。

.. gist:: https://gist.github.com/ymt2/2139b413c601b9f94f0bfde14deadb19


作った動機とか用途とか
----------------------

Wercker で CI しつつデプロイする先がさくら VPS 上の FreeBSD jail という構成のアプリを
デプロイのたびに再起動する方法を考えていて、uWSGI の touch-reload オプションみたいなのがあればできそうだなーと思いあたってそのまま実装した。


使いかた
--------


動作環境やら事前準備やら
~~~~~~~~~~~~~~~~~~~~~~~~


- Python 2.7
- Supervisor, watchdog インストール済み
- `touch_reload_event_listener.py`_ を適当な場所にダウンロード


設定
~~~~

Supervisor の conf ファイルに下記を記載して ``$ supervisorctl reload`` する。トリガーファイルを touch すると指定した program が再起動するはず。

今回は jail 内の Supervisor を調整したいのでこんな ``/usr/jails/{container}/home/{user}/{program}.reload`` 感じのパスを touch すると無事再起動された。あとは SSH しつつ叩くだけ。

.. code-block:: INI

   [eventlistener:touch_reload]
   command={Python 2.7} {touch_reload_event_listener.py のパス} -p {Supervisor 上の program_name} -f {トリガーファイルのパス}
   events=PROCESS_STATE


参考にしたもの
--------------

`superlance`_ の ``memmon.py`` を全力で参考にした。


.. author:: default
.. categories:: tips
.. tags:: FreeBSD,FreeBSD jail,Supervisor
.. comments::

.. _`touch_reload_event_listener.py`: https://gist.github.com/ymt2/2139b413c601b9f94f0bfde14deadb19
.. _`superlance` : https://github.com/Supervisor/superlance
