1.  How can I make sure that all desktop client connections are secured via TLS and are never allowed to even attempt plain text authentication?

This is the default for Haraka. In fact, there's not even an option to enable authentication without SSL. Here's how you can confirm this for yourself:

    % nc node.tnpi.net 25
    220 node.tnpi.net ESMTP Haraka 2.2.8 ready
    ehlo hi.there
    250-node.tnpi.net Hello servedby.tnpi.net [192.48.85.146], Haraka is at your service.
    250-PIPELINING
    250-8BITMIME
    250-SIZE 500000
    250 STARTTLS
    quit
    221 node.tnpi.net closing connection. Have a jolly good day.`

Note that there aren't any options there for authentication?  Now let's connect again with encryption:

    % openssl s_client -connect node.tnpi.net:25 -starttls smtp
    CONNECTED(00000003)
    depth=0 /C=US/ST=Washington/L=Seattle/O=TNPI/CN=node.tnpi.net/emailAddress=ma*t@t*pi.n*t
    verify error:num=18:self signed certificate
    verify return:1
    depth=0 /C=US/ST=Washington/L=Seattle/O=TNPI/CN=node.tnpi.net/emailAddress=ma*t@t*pi.*et
    verify return:1
    <snip lots of openssl certificate stuff>
    ---
    250 STARTTLS
    ehlo matt.simerson.net
    250-node.tnpi.net Hello servedby.tnpi.net [192.48.85.146], Haraka is at your service.
    250-PIPELINING
    250-8BITMIME
    250-SIZE 500000
    250 AUTH PLAIN LOGIN
    quit
    221 node.tnpi.net closing connection. Have a jolly good day.
    closed

Notice that now there's the option to AUTH. 