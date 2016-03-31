# OpenBSD-spamlogd
Modification of OpenBSD's spamlogd(8) that also updates TRAPPED entries in /var/db/spamd.
To activate, packets to spamd(8) must be logged to the same pflog(4) interface, in pf.conf(4) this means:

	-pass in     on egress proto tcp from any to any port smtp \
	-	divert-to 127.0.0.1 port spamd
	+pass in log on egress proto tcp from any to any port smtp \
	+	divert-to 127.0.0.1 port spamd

To download and apply the patch:

	REL=`uname -r|sed 's/\.//'`
	cd /tmp
	ftp https://raw.githubusercontent.com/bdijkstra82/OpenBSD-spamlogd/master/spamlogd${REL}.patch
	cd /usr/src
	patch -p0 < /tmp/spamlogd${REL}.patch

To build, install and run:

	cd /usr/src/libexec/spamlogd
	make obj
	make
	make install
	/etc/rc.d/spamlogd restart

###NOTES of importance casually placed at the very bottom of this document
* Before upgrading to a new version of OpenBSD, make sure to temporarily disable logging on the spamd port, or temporarily disable spamlogd itself to prevent unwanted interference from the stock spamlogd.
