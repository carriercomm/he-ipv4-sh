What
====

This is a maintenance script to ensure that your Hurricane Electric Tunnel Broker IPv6 tunnel is always listening for connections from your current external IPv4 address.  

This script is helpful if you have a dynamic IP address from your ISP.  The script is also logical and does not needlessly update the IPv6 address via HE\'s API.

This was written and tested with Debian Squeeze (6.0), but it may work on other Debian-derived distributions.  It was also written with the assumption that your tunnel is configured the same way as explained in my blog post:

- [The Blog of Tim Heckman: Hurricane Electric Tunnel Broker IPv6 Gateway](http://blog.timheckman.net/2011/05/24/he-tunnelbroker-ipv6-gateway/ "http://blog.timheckman.net/2011/05/24/he-tunnelbroker-ipv6-gateway/")

Set up
======

Set up of this script is easy.  You only need to save the script locally and configure some cronjobs for it.

1. Download the script and save it to /usr/local/sbin/he-ipv4.sh using either curl or wget:
 * ```curl -o /usr/local/sbin/he-ipv4.sh "https://raw.github.com/theckman/he-ipv4-sh/master/he-ipv4.sh"```
 * ```wget -O /usr/local/sbin/he-ipv4.sh "https://raw.github.com/theckman/he-ipv4-sh/master/he-ipv4.sh"```
2. Edit the file using your preferred text editor.  Make sure all configuration items are set properly.
3. Edit the root user's crontab (```crontab -e```) to run this script post reboots and every 15 minutes:

> ```@reboot /usr/sbin/he-ipv4.sh >/dev/null 2>&1```
> 
> ```*/15 * * * * /usr/sbin/he-ipv4.sh >/dev/null 2>&1```

Configuration
=============

The configuration is all done within the script file itself.  There is a configuration section near the top of the file.

* **userID**: this is your UserID value from the Main Page of HE's Tunnel Broker
* **userPass**: an MD5 hash of your Tunnel Broker password:
 * ```echo -n YourPassword | md5sum```
* **tunnelID**: the unique tunnel ID number from your tunnel's information page
* **tunnelName**: this is the *local* name for the Tunnel Broker interface.  In my blog I use the name "he-ipv6"
* **LOG**: the prefix for syslog entries
* **LVL**: defines the verbosity of the logging of the script (see the config section for more info)
* **URLA/URLB**: the two URLs that we alternate pulling external IPv4 address from. This *should* not be modified.
