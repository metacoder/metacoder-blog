---
title: "Kommentare in IPTables"
authors:
  - Felix Becker
date: 2012-12-29T13:41:20+02:00
lastmod: 2012-12-29T14:07:09+02:00
---

Ich habe bei einer Maschine IPTables für IPv6 konfiguriert und dabei festgestellt, dass die Rules sehr schnell unübersichtlich werden, gerade wenn man verschiedene Netze in IPTables freischaltet. Bei der Suche nach der besten Dokumentationsmöglichkeit bin ich im [Racker Hacker Blog](https://major.io/2010/07/26/adding-comments-to-iptables-rules/) auf den Hinweis gestoßen, dass IPTables Modul "comment" gibt, das einen Kommentar direkt beim erstellen einer Rule setzt. Dieser Kommentar ist dann auch bei `iptables -L` sichtbar:

#### Rule erstellen:

```plain {linenos=false}
ip6tables -I INPUT --src 2001:1620:f77::/48 -i eth0 -j ACCEPT -m comment --comment "Supertux home sixxs net"
```

#### Ausgabe ip6tables -n -L

```plain {linenos=false}
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     all      ::/0                 ::/0                state RELATED,ESTABLISHED
ACCEPT     all      ::/0                 ::/0
ACCEPT     icmpv6    ::/0                 ::/0                /* required for ipv6 to work! */
ACCEPT     all      2001:1620:f77::/48   ::/0                state NEW /* Supertux home sixxs net */
ACCEPT     all      20a1:4f8:162:4ff5::/64  ::/0                state NEW /* hetzner vm net */
ACCEPT     all      2001:6f8:900:7ea::2/128  ::/0                state NEW /* notebook sixxs tunnel */
ACCEPT     all      2001:4dd0:ff00:108d::2/128  ::/0                state NEW /* Olymp sixxs tunnel */
ACCEPT     all      2a01:4f8:161:3fff:200::2/128  ::/0                state NEW /* remote coding4coffee */
REJECT     all      ::/0                 ::/0                reject-with icmp6-port-unreachable

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
REJECT     all      ::/0                 ::/0                reject-with icmp6-port-unreachable

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

Die Kommentare bleiben nach einem "`service ip6tables save`" auf einem Centos/RHEL natürlich bestehen. Ein besserer Weg um seine Firewall-Rules zu dokumentieren ist mir bisher noch nicht begegnet.
