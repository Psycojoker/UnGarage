RSS
===

Rsstail
-------

We are going to use `rsstail <http://www.vanheusden.com/rsstail/>`_
to retrieve our FriendTimeLine. It's a small software that read a
RSS (or an atom feed) like a tail -f. But, in rsstail default
formating, the url and the title of the RSS item are put on 2 lines
instead of one. I don't like that, so I patch rsstail's source
code, here is the diff (yes, this is **optional**):

::

    430c430
    <                                       printf(" %s", heading);
    ---
    >                                       printf("%s ", heading);
    434c434
    <                                       printf("%s%s\\\\n",
    no\_heading?" ":"Title: ", item\_cur -> title);
    ---
    >                                       printf("%s%s",
    no\_heading?"":"Title: ", item\_cur -> title);

If you don't care, just install it using your pkg manager.

For debian(-based):

::

    apt-get install rsstail
