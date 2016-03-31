# OpenBSD-spamlogd
Modification of OpenBSD's spamlogd(8) that also updates TRAPPED entries in /var/db/spamd. Requires OpenBSD 5.8 or higher with source tree. Tested on OpenBSD 5.9.

To download and apply the patch:

	cd /tmp
	ftp https://raw.githubusercontent.com/bdijkstra82/OpenBSD-spamlogd/master/spamlogd.patch
	cd /usr/src
	patch -p0 < /tmp/spamlogd.patch

Now you can read the manpage:

	mandoc libexec/spamlogd/spamlogd.8 | less

To build, install and run:

	cd /usr/src/libexec/spamlogd
	make obj
	make
	make install
	/etc/rc.d/spamlogd restart

At this point, spamlogd should behave as before.
To activate the new feature, packets to spamd(8) must also be logged (and on the same pflog(4) interface).
In pf.conf(4) this means:

	-pass in     on egress proto tcp from any to any port smtp \
	-	divert-to 127.0.0.1 port spamd
	+pass in log on egress proto tcp from any to any port smtp \
	+	divert-to 127.0.0.1 port spamd

And:
	pfctl -f /etc/pf.conf

###NOTES of importance casually placed at the very bottom of this document
* Before upgrading to a new version of OpenBSD, make sure to temporarily disable logging on the spamd port, or temporarily disable spamlogd itself to prevent unwanted interference from the stock spamlogd.
