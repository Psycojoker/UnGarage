Misc
====

Some useful stuff
-----------------


-  Tinyfiying an url with ur1.ca:

  ::

     curl -s http://ur1.ca/ -d "longurl=the url" | sed -u -e '/Your ur1/!d;s: *::g;s:<[^>]*>::g;s:^.*Yourur1is\\:h:h:;'

-  `The status.net api <http://status.net/docs/api/>`_.
-  `Twitter api <http://apiwiki.twitter.com/w/page/22554648/FrontPage>`_.
-  Url to have the atom feed of a research on twitter:
       http://search.twitter.com/search.atom?q=pouet

-  `IceRocket <http://www.icerocket.com/>`_ help you build
   `complexes query <http://www.icerocket.com/search?tab=twitter&lng=&q=this+OR+that+OR+thuse&x=0&y=0>`_
   on searching twitter and provide an
   `RSS <http://www.icerocket.com/search?tab=twitter&q=this+OR+that+OR+thuse&rss=1>`_
   for those queries.


[Bonus] Pipe a RSS to status.net/twitter
----------------------------------------

You can pipe the output of rsstail to an status.net/twitter account
with bti. This is an easy and stable way to post a RSS feed to
status.net/twitter.

Here is an example:

::

    rsstail -u http://example.com/rss.xml -n 0 -l -N -z -P -i 30 -Z "[an header]" | bti

**Warning, since it reads a RSS, it is very sensible, any change in the title or in the url will create a new post on your status.net/twitter account.**

