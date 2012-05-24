Setting up the bot
==================

ii
--

Everything we are going to do will be based on
`ii <http://tools.suckless.org/ii>`_. It's an irc file-system that
simplify a lot all the interactions with irc. The suckless page
describe the principle.

For debian(-based):

::

    apt-get install ii

we'll need a reconnection file to keep ii up, here is mine with 2
optionals way to join the irc chan(s):

::

    #!/bin/bash
    while true
    do
        # if you only have one chan
        (sleep 5; echo "/j #my\_chan" > path/to/the/server/in) &
        # if you have multiple chans and want to add new chans easily (you just need to do an "echo chan\_name >> chans"
        fori in \`cat chans\`
        do (sleep 5; echo "/j $i" > path/to/the/server/in) &
        done
        ii \\\\
            -i "folder\_where\_ii\_will\_put\_is\_stuff" \\\\
            -s "irc.my\_server\_addr.pouet" \\\\
            -p "port number of the server" \\\\
            -n "bot name" \\\\
            -f "bot name"
    done

(Don't forget to put the correct informations)

Just launch the script (chmod +x && ./script or sh script). We now
have our irc bot.
