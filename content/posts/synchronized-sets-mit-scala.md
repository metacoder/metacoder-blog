---
title: "Synchronized Sets mit Scala"
authors:
  - Felix Becker
date: 2012-12-11T07:14:18+02:00
lastmod: 2012-12-11T07:16:45+02:00
---

Ein Snippet zum Frühstück:

Bei mir hat sich gerade die Notwendigkeit ergeben, in Scala mit einem mutable HashSet in einem Multithreading-Context zu arbeiten.

Um dieses Threadsafe zu machen kann man das HashSet mit dem Trait SynchronizedSet instanzieren:

```scala {linenos=false}
val mutableThreadsafeSet = new HashSet[Long] with SynchronizedSet[Long]
```

Ich liebe diese Sprache <3
