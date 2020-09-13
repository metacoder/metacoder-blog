---
title: "Gentoo - Chromium - kaputte Fonts"
authors:
  - Felix Becker
date: 2014-03-09T11:11:25+02:00
lastmod: 2016-04-20T19:56:52+02:00
---

Ich habe seit einiger Zeit Probleme mit Chromium auf Gentoo und der Fontdarstellung.

Die Fonts "Verdana, Helvetica, Arial, sans-serif" (CSS) sahen in normaler Textgröße super aus, sobald aber die Font größer war (z.B. 20px font size), war die Font pixelig und verwaschen.

[![](/posts/images/chromium-fontbroken-250px.png)](/posts/images/chromium-fontbroken.png)

Um das Problem zu lösen, habe ich mir das Corefonts-Package mit

```plain {linenos=false}
emerge media-fonts/corefonts
```

merged. Seit der Installation sehen auch alle Schriften in größerer Version wieder sauber aus.

Ich muss noch mal debuggen, wie der Font-Loading-Mechanismus vom Chrome funktioniert um zu sehen, welche Schriftarten vorher die Übeltäter waren.
