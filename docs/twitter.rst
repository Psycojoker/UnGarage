Twitter/Status.net
==================

Bti
---

We also need bti to post our tweet. You can find it
`here <http://gregkh.github.com/bti/>`_ (the last version needs
`liboauth <http://liboauth.sourceforge.net/>`_ to be able to post
on twitter. I'll explain this later).

Status.net
----------

Let's start with status.net, since this is fucking much easier than
with twitter (stupid Oauth).

Getting the FriendTimeLine
~~~~~~~~~~~~~~~~~~~~~~~~~~

(The FriendTimeLine is all your friend dents + your dents).

This is really easy, just do:

::

    rsstail -u
    http://identi.ca/api/statuses/friends\_timeline/your\_user\_name.rss
    -N -z -P -l -i 120 -n 0 > path/to/the/irc/server/\\\\#the\_chan/in

Yes, jut one line of bash and yes, it is really easy to transform
this into a multi account status.net client.

For the options, *-N* remove "title: " and "link :" from the
default formating. *-z* and *-P* is to avoid quitting if errors
occurs. *-l* show the links. *-i 120* is the sleeping time between
checking for news updates. *-n 0* is the number of item to show at
startups, 0 means nothing will appears except the new ones.

(You can ind those links here:
`http://identi.ca/api/statuses/friends\_timeline/your\_user\_name.rss <http://identi.ca/api/statuses/friends_timeline/your_user_name.rss>`_)

Since not all software are perfect, rsstail might stop working
after few days (dunno why), here is a quick fix to keep it up:

::

    echo "true" > .while\_cond #only once
    while true; do rsstail -u
    http://identi.ca/api/statuses/friends\_timeline/your\_user\_name.rss
    -N -z -P -l -i 120 -n 0 > path/to/the/irc/server/\\\\#the\_chan/in;
    if [ $(cat .while\_cond) = "false" ]; then break; fi; done

This will keep rsstail up forever. If you want to stop it, just do
an *echo "false" > .while\_cond* and kill the rsstail instance
(*ps aux | grep rsstail* etc...).
**Don't forget to do an *echo "true" > .while\_cond* after that to avoid breaking the reboot mechanism of your other rsstail instances.**

One last (dirty) trick to have an header with colors if you wish to
distinguish multiple feeds easily:

::

    rsstail -u http://identi.ca/api/statuses/friends\_timeline/your\_user\_name.rss -N -z -P -l -i 120 -n 0 -Z '\\\\e[35;4m@your\_user\_name:\\\\e[0m' | while read -r line; do echo -e "$line" > path/to/the/irc/server/\\\\#the\_chan/in; done

Posting
~~~~~~~

To post we need to parse the output of the irc chan. Here is a
small script that do that, the command is *!twitter*:

Warning: dirty code.

::

    #!/bin/bash
    CHAN="#your\_irc\_chan"
    tail -n 0 -f path/to/the/server/$CHAN/out | grep -v --line-buffered "<bot\_name>" | grep --line-buffered "\\\\>" \| while read -r line
    do
        # debug
        printf  '%s\\\\n' "$line"
        message\_text=\`printf '%s\\\\n' "$line" | sed 's/.\\\\+> //'\`
        case $message\_text in         !debug)
              echo "working!" >> path/to/the/server/$CHAN/in ;;
            !twitter\\\\ \*)
                temp=\`printf '%s\\\\n' ""$message\_text"" | sed 's/!twitter \\\\?//'\`
                printf '%s\\\\n'  "$temp" | bti --account user\_name --password your\_password --action update
                if [ ${#temp} -lt 141 ]
                then echo "Grand succès !" > /path/to/the/server/$CHAN/in
                else
                    echo "Big tweet, might not work (${#temp} chars)" > path/to/the/server/$CHAN/in
                    # here, I still post because identica automaticaly tinyfied the
                    # urls he received and I was to lazy to wrote a way to
                    # calculate the effective size of a dent with urls in it
                fi ;;
        esac
    done

As you can see, this script is really simple and very extensible.
you can add as much accounts as you want and as much commands as
you want. Congratulation, you now have a multi-account status.net
client in just a few lines of bash.

*If you wrote more code use printf + '' instead of echo to avoid execute of malicious code coming from irc.*

`The status.net api <http://status.net/docs/api/>`_ with curl
examples that you can easily add to the previous script.

And voilà, you have everything you need to build your own
status.net client.

Twitter
-------

Twitter is really similar to status.net at the exception that they
have this stupid oauth. Here are the tricks to works with oauth:

Posting
~~~~~~~

If it's you identi.ca/status.net account that post on twitter, jump
this part and just post on the identi.ca one as specified before.

**You'll need bti compiled with `liboauth <http://liboauth.sourceforge.net/>`_.**

*If you install liboauth from sources on a debian, don't forget the --prefix=/usr.*

This is the easiest part of working with twitter, you just have to
configure bti according to
`this blog post <http://gluegadget.com/blog/index.php?/archives/34-Twitter,%20-OAuth-and-bti.html>`_
(the last part of the post).

Getting the FriendTimeLine
~~~~~~~~~~~~~~~~~~~~~~~~~~

Now, the dirty part. You'll need
`twurl <https://github.com/marcel/twurl>`_, it is a curl with the
oauth support, and a webserver where you can place a static file.
If you don't have a webserver, you can simple do:

::

    python -m SimpleHTTPServer

This will launch a basic http server that will display the current
folder.

**If you are running a debian lenny, you'll need the lasted versions of rubygems (not the one in the pkgs) to make twurl works.**

Twurl README explain how to register it to twitter.

Now, you can get your FriendTimeLine. Just do:

::

    twurl /1/statuses/home\_timeline.xml > path/to/your/httpserver/folder/twitterfriendtimeline.xml

I don't know why, but this command has refused to works in crontab,
so I've simply do a (in a screen):

::

    while true; do twurl /1/statuses/friends\_timeline.rss > path/to/your/httpserver/folder/twitterfriendtimeline.xml; if [ $(cat .while\_cond) = "false" ]; then break; fi; sleep 120; done

Now you have your FriendTimeLine accessible by rsstail and you can
use the same solution than for status.net.

