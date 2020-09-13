---
title: "Steam / X-Plane auf Linux: Pavucontrol erlaubt Umschalten des Output devices nicht"
authors:
  - Felix Becker
date: 2013-06-09T04:28:42+02:00
lastmod: 2013-06-09T18:00:03+02:00
---

Sowohl unter Fedora als auch unter Gentoo (Stable) hatte ich das Problem, dass ich mit Pavucontrol für das laufende Spiel das Ausgabegerät nicht umschalten konnte. In Pavucontrol konnte ich zwar den entsprechenden Button drücken und mein Wunschausgabegerät wählen, allerdings hat dies keine Auswirkung gehabt und das Ausgabegerät wurde nicht umgestellt.

Der nächste Versuch das Ganze über die command line zu lösen hat mehr Licht ins Dunkle gebracht:

```bash {linenos=false}
# IDs der Ausgabegeräte rausfinden
pactl list sinks

# Applikationen die über PulseAudio Sound ausgeben auflisten:
pactl list sink-inputs

# Ausgabe von X-Plane (Sink-Input 118) auf Sink 1 (neues Ausgabegerät) umleiten:
pactl move-sink-input 118 1
```

Der "`pactl move-sink-input 118 1`"-Befehl terminierte bei mir mit "`Failure: Invalid argument`". Beim "`pactl list sink-inputs`" ist mir beim X-Plane folgende Zeile aufgefallen:

```plain {linenos=false}
flags: DONT_MOVE START_CORKED FIX_RATE
```

Das Flag `DONT_MOVE` hat verhindert, dass man das Ausgabegerät umschalten kann. X-Plane und Steam verwenden OpenAL für die Soundausgabe. [Im Steamforum](https://steamcommunity.com/app/93200/discussions/0/864959809826195633/?l=german) habe ich die Lösung für dieses Problem gefunden. Man muss einfach die Datei `.alsoftrc` in seinem Home-Verzeichnis mit folgendem Inhalt anlegen:

```cfg {linenos=false}
# ~/.alsoftrc
[pulse]
allow-moves=yes
```

Ob das irgendwo anders zu Problemen führt / warum OpenAL by default keine Moves erlaubt kann ich aber noch nicht mit Sicherheit sagen. X-Plane funktioniert auf jeden Fall tadellos mit den neuen Settings.
