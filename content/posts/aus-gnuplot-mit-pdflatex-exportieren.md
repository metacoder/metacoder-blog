---
title: "Aus gnuplot mit pdflatex exportieren"
authors:
  - Nis Meinert
date: 2013-06-04T22:58:43+02:00
lastmod: 2013-06-08T11:37:40+02:00
---

Ein recht nützliches Skript für alle gnuplot Freunde. Es exportiert euren Plot in ein Tex-File und kompiliert es gleich zu einem PDF. Damit habt ihr auch in gnuplot Zugriff auf den vollen Latex-Formelsatz, zur Not sind auch nachträgliche Änderung im Tex-File möglich (dort würde sich dann z.B auch die Schriftart /-größe anpassen lassen). Standardmäßig ist der Export schwarz-weiß.

```gnuplot
save "$0.gp"
set terminal push
set terminal epslatex standalone

set output "$0.tex"
replot
set output
set terminal pop

system "pdflatex $0.tex > /dev/null"
system "rm $0.aux $0.log"
```

Der Aufruf des Skripts sieht wie folgt aus:

```gnuplot {linenos=false}
call "export.gp" "meinTollerPlot"
```
