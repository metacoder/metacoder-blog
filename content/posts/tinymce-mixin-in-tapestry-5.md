---
title: "TinyMCE Mixin in Tapestry 5"
authors:
  - Felix Becker
date: 2012-06-29T00:23:18+02:00
lastmod: 2012-06-29T01:19:55+02:00
---

Um das Schreiben von Blog-Einträgen komfortabler zu gestalten haben wir [TinyMCE](http://www.tinymce.com/) in unser Blog eingebunden. Für TinyMCE müssen die TinyMCE-Javascripts in der Seite geladen und dann ein tinyMCE.init aufgerufen werden.

Bei Tapestry bietet sich hierfür ein Mixin an um nicht auf jeder Seite den Import der JS-Lib und den Init-Call einbauen zu müssen.

Das Mixin wird in Tapestry-Manier im Package "mixins" erstellt und heißt bei uns TinyMCE.

```java
package de.metacoder.blog.mixins;

import org.apache.tapestry5.MarkupWriter;
import org.apache.tapestry5.annotations.BeginRender;
import org.apache.tapestry5.annotations.Import;
import org.apache.tapestry5.annotations.InjectContainer;
import org.apache.tapestry5.corelib.components.TextArea;

@Import(library = { "context:scripts/tiny_mce/tiny_mce.js" })
public class TinyMCE {

        @InjectContainer
        private TextArea textArea;

        @BeginRender
        public void enableTinyMCE(final MarkupWriter markupWriter) {
                markupWriter.writeRaw(
                        "<script type=\"text/javascript\">\n" +
                        "       tinyMCE.init({\n" +
                        "               mode : \"exact\",\n" +
                        "               elements: \"" + textArea.getClientId() + "\"\n" +
                        "       });\n" +
                        "</script>"
                );
        }
}
```

Sobald man dieses Mixin erstellt hat, kann man es im Template bei einer Textarea als Mixin angeben:

```xml {linenos=false}
<t:textarea t:mixins="tinyMCE" t:id="content" t:value="blogEntry.content" />
```

Das sorgt dann automatisch dafür, dass die TinyMCE-Scripts in der Seite richtig geladen und der Editor initialisiert wird.
