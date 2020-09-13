---
title: "SBT Resolvers überschreiben"
authors:
  - Mathias Kub
date: 2016-04-20T19:45:44+02:00
lastmod: 2016-04-20T20:13:33+02:00
---

In SBT gibt es die Möglichkeit **resolvers** und **externalResolvers** zu überschreiben. Auch wenn dies geschieht, werden immer vorher die Typesafe-Mirrors (die leider relativ langsam sind) verwendet. Weiters kann es vorkommen, dass ein Zugriff auf Artefakte nur über einen internen Proxy (Nexus, Artifactory,...) möglich ist.

Um das Problem zu lösen, müssen die **fullResolvers** überschrieben werden. Vor den gewünschten Repositories müssen **projectResolver** und **otherResolvers** verwendet werden.

In der **build.sbt** wird dazu folgender Eintrag ergänzt:

```scala
resolvers := Seq(
  "maven-proxy" at "https://your.artifactory/maven-proxy/",
  Resolver.url("ivy-proxy", url("https://your.artifactory/ivy-proxy/"))(Resolver.ivyStylePatterns)
)

fullResolvers := Seq(projectResolver.value) ++ otherResolvers.value ++ resolvers.value
```

Für mehrere Repositories bzw. bessere Lesbarkeit können für die Repository-Deklarationen auch Hilfsmethoden geschrieben werden, z.B.:

```scala
def maven(repoName: String, repoUrl: String): MavenRepository = repoName at repoUrl
def ivy(repoName: String, repoUrl: String): URLRepository = Resolver.url(repoName, url(repoUrl))(Resolver.ivyStylePatterns)

resolvers := Seq(
  maven("maven-proxy", "https://your.artifactory/maven-proxy/"),
  ivy("ivy-proxy", "https://your.artifactory/ivy-proxy/")
)

fullResolvers := Seq(projectResolver.value) ++ otherResolvers.value ++ resolvers.value
```

## Änderung für alle SBT-Projekte auf dem lokalen PC

Um diese Änderung für SBT auf dem lokalen PC für alle Projekte zu übernehmen, muss das gleiche in der **~/.sbt/0.13/global.sbt hinzugefügt werden**.

## Konfiguration für Play-Projekte

#### Proxy-Repository

Für Play müssen auf dem Proxy zusätzlich zu den Standard-Maven-Repositories (https://repo1.maven.org/maven2) folgende Repositories konfiguriert werden:

**maven-proxy (Maven Repository)**

* typesafe-releases: https://repo.typesafe.com/typesafe/releases/
* bintray-sbt-maven-releases: https://dl.bintray.com/sbt/maven-releases
* bintray-typesafe-maven-releases: https://dl.bintray.com/typesafe/maven-releases
* bintray-scalaz-releases: https://dl.bintray.com/scalaz/releases/

**ivy-proxy (Ivy Repository)**

* bintray-sbt-plugin-releases: https://dl.bintray.com/sbt/sbt-plugin-releases/
* bintray-sbt-native-packages: https://dl.bintray.com/sbt/native-packages
* bintray-typesafe-sbt-plugins: https://dl.bintray.com/typesafe/sbt-plugins
* bintray-typesafe-ivy-releases: https://dl.bintray.com/typesafe/ivy-releases

#### Play-Projekt

Die Änderung der Resolver muss in der **build.sbt** sowie in der **project/plugins.sbt** vorgenommen werden, um diese für beide Builds zu aktivieren.

Getestet mit einem Play 2.5 Scala Projekt.
