========================================
 ChatWork + Fluidで未読件数の表示をする
========================================


`Fluidで作ったchatwork.comアプリで未読表示`_

`Fluid`_ でユーザスクリプトが使えることを知ってちょろっと探してみると上の記事が見つかったので。


Fluidって？
===========

WebアプリのURLを食べさせるとMacOSのhoge.appを吐き出してくれるものです。ユーザスクリプトはPurchaseしないと利用できない模様。


元の記事と何が違うの？
======================

未読件数の表示に加えて、Docのアイコンが跳ねます。changelogを見ると`window.fluid.requestUserAttention`ってAPIがあったのでこれや！と思って1行追加しただけです。
`Fluid Developer`_ 見ても書いてなかったぞ。。。

.. gist:: https://gist.github.com/ymt2/4270013

.. author:: default
.. categories:: tips
.. tags:: fluid,chatwork,javascript,mac os x
.. comments::

_`Fluidで作ったchatwork.comアプリで未読表示`: http://www.simplegimmick.com/2012/02/fluidchatworkcom.html
_`Fluid`: http://fluidapp.com/
_`Fluid Developer`: http://fluidapp.com/developer/
