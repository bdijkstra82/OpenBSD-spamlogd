# OpenBSD-spamlogd
Modification of OpenBSD's spamlogd(8) that also updates TRAPPED entries in /var/db/spamd.  To activate, packets to spamd(8) must be logged to the same pflog(4) interface, for example:

pass in log on egress proto tcp from any to any port smtp \
        divert-to 127.0.0.1 port spamd


