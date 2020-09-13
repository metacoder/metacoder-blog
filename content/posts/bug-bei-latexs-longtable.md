---
title: "Bug bei LaTeXs longtable"
authors:
  - Nis Meinert
date: 2013-06-05T19:12:13+02:00
lastmod: 2013-06-05T22:28:58+02:00
---

Ich weiß nicht, ob das wirklich ein Bug ist, ich würde es aber mal zumindest unter _unexpected behavior_ packen:

Folgender Code crasht:

```latex
\begin{longtable}{r||r|r|r}
\caption{Messwerte 2 - 10 Messwerte}\\[3mm]
\label{tab:mv2}

Nr. & Impulse & Brutto & Netto\\
```

Schuld daran ist Zeile 3 in der sich hinter `}` noch Leerzeichen einschummeln. Ein abschließendes `%` fixt das:

```latex {linenos=false}
\label{tab:mv2}%
```
