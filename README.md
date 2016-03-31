# OpenBSD-spamlogd
Modification of OpenBSD's spamlogd(8) that also updates TRAPPED entries in /var/db/spamd.
To activate, packets to spamd(8) must be logged to the same pflog(4) interface, for example:

	pass in log on egress proto tcp from any to any port smtp \
		divert-to 127.0.0.1 port spamd

To download and apply the patch:

	REL=`uname -r|sed 's/\.//'`
	cd /tmp
	ftp https://raw.githubusercontent.com/bdijkstra82/OpenBSD-spamlogd/master/spamlogd${REL}.patch
	cd /usr/src
	patch -p0 < /tmp/spamlogd${REL}.patch

To build and install:

	cd /usr/src/libexec/spamlogd
	make obj
	make
	make install
