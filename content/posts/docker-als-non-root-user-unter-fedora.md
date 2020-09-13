---
title: "Docker als non-root User unter Fedora"
authors:
  - Mathias Kub
date: 2015-03-15T01:33:04+02:00
lastmod: 2015-03-15T01:33:04+02:00
---

Um Docker als nicht-root Benutzer unter Fedora verwenden zu können, muss der Benutzer zur Gruppe **dockerroot** hinzugefügt und in die Datei **/etc/sysconfig/docker** zur **OPTIONS**-Variable **-G dockerroot** hinzugefügt werden.

Die Gruppe **dockerroot** wird bei der Installation erstellt.

Fedora 21 Docker 1.5.0

#### Kommentar von [Felix Becker]({{< ref "/authors/felix-becker" >}}) am 2015-07-05 20:38:15

Sollte man nicht tun. Ein `docker run --privileged` kann erheblichen Schaden damit anrichten - das wäre analog zu sudo mit NOPASSWD: ALL.  
Dann lieber `alias docker="sudo docker"`.
