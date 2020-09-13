---
title: "IPv6 Privacy Extension und NetworkManager"
authors:
  - Mathias Kub
date: 2014-02-01T17:40:49+02:00
lastmod: 2014-02-01T18:23:59+02:00
---

Um die IPv6 Privacy Extension und NetworkManager gleichzeitig zu verwenden, müssen in **/etc/sysctl.conf** die Zeilen

```cfg
net.ipv6.conf.all.use_tempaddr = 2
net.ipv6.conf.default.use_tempaddr = 2
net.ipv6.conf.nic0.use_tempaddr = 2
```

hinzugefügt werden, wobei **nic0** mit dem Namen des Netzwerkinterfaces ersetzt werden muss.

NetworkManager liest /etc/sysctl.conf (nicht aber /etc/sysctl.d/*.conf) ein.

Um die Extension für eine bestimmte Verbindung zu aktivieren, muss die Zeile

```cfg {linenos=false}
IPV6_PRIVACY=rfc3041
```

in **/etc/sysconfig/network-scripts/ifcfg-nic0** hinzugefügt werden (nic0 wieder mit der Verbindung ersetzen).

Bei mir entstand die Verwunderung, wieso es keine ifcfg-wlp3s0 Konfigurationsdatei gibt (wlp3s0 = Name meines WLAN-Interfaces). Das kommt daher, dass wlp3s0 keine _Verbindung_ (sondern ein Interface) ist.

Deswegen gibt es auch für jede WLAN-Verbindung (jedes Netwerk) eine Konfiguration - auch für die Ethernet-Verbindung.

Siehe auch:
* https://wiki.archlinux.org/index.php/IPv6#Privacy_extensions
* https://fedoraproject.org/wiki/Tools/NetworkManager/IPv6#IPv6_Privacy_Extensions
