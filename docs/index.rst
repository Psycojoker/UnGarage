.. UnGarage documentation master file, created by
   sphinx-quickstart on Thu Apr 12 01:01:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to UnGarage's documentation!
====================================

This is a guide on how I'm managing the irc bot UnGarage for `la Quadrature du
Net <https://laquadrature.net>`_ (and many others that I've deployed
afterwards). My constrain are the modular, changing and original requests of a
`strange mustache owner
<http://upload.wikimedia.org/wikipedia/commons/c/c7/J%C3%A9r%C3%A9mie_Zimmermann_-_Journ%C3%A9e_du_domaine_public_2012_n02.jpg>`_.
Therefor I was in need of a very flexible framework than can catch up with this
strange phenomena.

The result of this work make an heavy use of the UNIX principle of a collection
of small reusable tools. Because of this I can't release a source code so I'm
releasing this documentation.

Using this method is:

-  a very high flexibility, you'll be able to modify, stop, restart and change
   every part of it independently at any time

-  it is language agnostic, program it in whatever you desire (but you'll very
   likely need at least a bit of shell script to glue everything together)

-  make the problem of creating a multitasking IRC boot so simple and elegant

-  very simple to use and maintained ... once you dig in it (I found this
   easy but this isn't the common way of approaching this kind of problem)

-  might be a bit hard for other people to help you or understand your work for
   the same reason than described in the previous point

-  is annoying if you need to relaunch everything (eg: server reboot) but I
   think I'm about to solve this problem (using `supervisord
   <http://supervisord.org/>`_) but I'm still experimenting

**This doc is in the process of a rewriting, everything after this might
be a bit outdated (but still works)**

.. toctree::
   :maxdepth: 2

   ii.rst
   rss.rst
   twitter.rst
   ngircd.rst


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

---

Send me `an email </about/about.html>`_ if you have any comment
(ie: correcting my horrible globish).


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

