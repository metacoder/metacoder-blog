---
title: "TinyMCE Mixin in Tapestry 5 - Version 2"
authors:
  - Felix Becker
date: 2012-07-08T19:35:26+02:00
lastmod: 2012-07-08T19:36:40+02:00
---

In der ersten [Version des TinyMCE-Mixins]({{< ref "/posts/tinymce-mixin-in-tapestry-5" >}}) wurde der der Javascript-Code der zum laden benötigt wird noch über den MarkupWriter in der @BeginRender-Methode in den Code geschrieben. Tapestry 5 stellt für das Ausführen von Javascript-Code in einer Component den "JavaScriptSupport" (injectbares Interface) zur verfügung. Über diesen Javascript-Support kann man JS-Code in die Seite schreiben der dann an der richtigen Stelle geladen wird. Man braucht so keine &lt;script&gt;-Tags mehr und Tapestry stellt sicher, dass das Javascript an der richtigen Stelle aufgerufen wird.

In der ersten Version wurde auch der gesamte Init-Javascript-Code (Mehrzeiler) in den Markup-Writer geschrieben. Es bietet sich an, diesen Code noch mal durch eine einfache Funktion zu ersetzen, die man in einer externen Javascript-Datei definiert die dann von Tapestry geladen (und komprimiert etc.) werden kann.

JS-Datei:

```javascript
function enableMCEAsMixin(element){
    tinyMCE.init({
        mode : "exact",
        theme_advanced_resizing : true,
        elements: element
    });
}
```

TinyMCE-Mixin:

```java
package de.metacoder.blog.mixins;

import org.apache.tapestry5.annotations.BeginRender;
import org.apache.tapestry5.annotations.Environmental;
import org.apache.tapestry5.annotations.Import;
import org.apache.tapestry5.annotations.InjectContainer;
import org.apache.tapestry5.corelib.components.TextArea;
import org.apache.tapestry5.services.javascript.JavaScriptSupport;

@Import(library = {
        "context:scripts/tiny_mce/tiny_mce.js",
        "context:scripts/tiny_mce/tiny_mce_mixin_init.js"
        })
public class TinyMCE {

    @InjectContainer
    private TextArea textArea;

    @Environmental
    private JavaScriptSupport javaScriptSupport;

    @BeginRender
    public void enableTinyMCE() {
        javaScriptSupport.addScript("enableMCEAsMixin('%s');", textArea.getClientId());
    }
}
```
