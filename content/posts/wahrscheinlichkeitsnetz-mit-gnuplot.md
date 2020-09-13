---
title: "Wahrscheinlichkeitsnetz mit gnuplot"
authors:
  - Nis Meinert
date: 2013-06-04T19:48:11+02:00
lastmod: 2013-06-05T00:52:28+02:00
---

Ich musste heute Messreihen auf statistische Reinheit untersuchen, indem ich sie auf Wahrscheinlichkeitspapier auftrage und mit der theoretisch nach Gauß berechneten Gerade vergleiche.

Da wir von der Uni aus nur ein schlecht kopiertes Wahrscheinlichkeitsnetz zur Verfügung gestellt bekommen haben, wurde diese Arbeit natürlich auf gnuplot abgewälzt.

Dummerweise musste ich festellen, dass gnuplot nativ keine derartige Skalierung seiner Ordinate anbietet (wie es sie für logarithmische Skalierung gibt) und es auch nicht möglich ist, die Achsen nach beliebigen Funktionen zu skalieren. Daher muss folgender Trick herhalten:

Man belässt die Achsen in ihrer äquidistanten Unterteilung und skaliert einfach die Messwerte. Anschließend überschreibt man die Achsenbeschriftung mit eigenen Werten. Für die Berechnung der Werte bietet gnuplot die Funktion invnorm.

Der Vollständige workaround sieht dann wie folgt aus:

```gnuplot
set ytics ("0.1" invnorm(0.001), "1" invnorm(0.01), "5" invnorm(0.05), "10" invnorm(0.1), "20" invnorm(0.2), "30" invnorm(0.3), "40" invnorm(0.4), "50" invnorm(0.5), "60" invnorm(0.6), "70" invnorm(0.7), "80" invnorm(0.8), "90" invnorm(0.9), "95" invnorm(0.95), "99" invnorm(0.99), "99.9" invnorm(0.999))
set yrange [invnorm(0.0001):invnorm(0.9999)]
set grid
set key left
plot "..." using 1:(invnorm($2/100))
```

Es empfiehlt sich die Legende links zu setzen, da sich im Idealfall die Messwerte von unten links nach oben rechts verteilen werden.

Hier noch ein kleines Beispiel:

{{< youtube KZ83MO5h9to >}}
