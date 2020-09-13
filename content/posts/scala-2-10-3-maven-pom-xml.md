---
title: "Scala 2.10.3 + Maven: pom.xml"
authors:
  - Felix Becker
date: 2014-01-05T04:53:40+02:00
lastmod: 2014-01-05T04:59:25+02:00
---

Anbei zum Kopieren die minimale pom.xml (+JUnit) mit der man sofort loslegen kann.

Die Scala-Sourcen werden dabei aus "_src/main/scala_", die Scala-Testsourcen in "_src/test/scala_" geladen. Parallel können ganz normal in "_src/main/java_" / "_src/test/java_" die entsprechende Java-Sourcen abgelegt werden.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemalocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelversion>4.0.0</modelversion>
    <groupid>de.metacoder</groupid>
    <artifactid>scalamaven</artifactid>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupid>org.scala-lang</groupid>
            <artifactid>scala-library</artifactid>
            <version>2.10.3</version>
        </dependency>
        <dependency>
            <groupid>junit</groupid>
            <artifactid>junit</artifactid>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupid>org.scala-tools</groupid>
                <artifactid>maven-scala-plugin</artifactid>
                <version>2.15.2</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>compile</goal>
                            <goal>testCompile</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <jvmargs>
                        <jvmarg>-Xms64m</jvmarg>
                        <jvmarg>-Xmx1024m</jvmarg>
                    </jvmargs>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

Der "configuration"-Part, der die JVM-Args steuert und den Heapspace setzt, ist optional. Bei kleineren Projekten reichen die default settings, man kann den Part also weglassen. Bei größeren Scala-Projekten benötigt man die Heapspace-Settings für den Compiler aber wieder, da die Defaults zu knapp werden.
