---
title: "nginx IPv6: Cannot assign requested address"
authors:
  - Benjamin Neff
date: 2014-03-29T19:10:15+02:00
lastmod: 2014-03-29T19:12:26+02:00
---

Ich hatte gerade das Problem, dass mein nginx beim Booten nicht gestartet hat, direkt danach hat das starten aber mit ssh problemlos funktioniert.  
Nachdem ich das "`2> /dev/null`" im init-script entfernt hab, hab ich dann beim Booten endlich den Grund dafür gesehen:

```plain {linenos=false}
nginx: [emerg] bind() to [2a01:4f8:201:910b:500::100]:80 failed (99: Cannot assign requested address)
```

Das kommt daher, weil ich für verschiedene vHosts eigene IPv6-Adressen konfiguriert hab, also auf jeder IPv6-Adresse läuft nur ein vHost. Das erklärt jetzt auch, wieso ich das Problem auf einem anderen Server nicht hab, weil da überall `[::]` konfiguriert ist.

Die Ursache für das Problem ist aber, dass direkt nach dem hinzufügen einer IP-Adresse diese noch im 'tentative state' ist, und deswegen nginx noch nicht drauf binden kann. Deswegen hab ich jetzt unter Debian in die `/etc/network/interfaces` beim `inet6` Block folgendes hinzugefügt:

```plain {linenos=false}
post-up echo -n "waiting for IPv6 addresses "; while ip -6 addr show tentative | grep . > /dev/null ; do echo -n "."; sleep 0.2 ; done; echo
```

Damit wartet er, bis alle IPv6-Adressen verfügbar sind, und nginx kann normal starten.
