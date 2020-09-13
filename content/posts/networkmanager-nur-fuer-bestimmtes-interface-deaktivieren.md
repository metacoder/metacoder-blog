---
title: "Networkmanager nur für bestimmtes Interface deaktivieren"
authors:
  - Nis Meinert
date: 2014-02-24T18:32:45+02:00
lastmod: 2014-02-24T18:32:45+02:00
---

Verhindert der Networkmanager ein manuelles setzen der IP (z.B. des eth0-Interfaces) reicht unter Ubuntu das hinzufügen von

```plain {linenos=false}
iface eth0 inet manual
```

in der **/etc/network/interfaces**-Datei um dieses Verhalten zu unterdrücken.
