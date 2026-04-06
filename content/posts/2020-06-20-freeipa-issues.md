---
title: "2020 06 20 Freeipa Issues"
date: 2020-06-21T14:09:12-04:00
draft: true
---

When i started down the path of running a homelab, I decided that I'd attempt
to run a full Linux domain to force myself into. At first, this worked out with
a single master node. After some time, i added a second master node with
two-way replication, DHCP failover, and dynamic DNS thrown in there too. Pretty
cool stuff once you get it working. Until, it fails... hard. 

FreeIPA runs a 389 server to provide directory services. My 2 nodes run on
CentOS 8 on a single ProxMox hypervisor at the time of writing this article.

TODO - insert image of replication here.

Upon, creating these server I've been through several iteratons; fighting
random problems with replication, dns failing, ldap timeouts.

Recently, one of the issues I've faced is a mix of all three.
