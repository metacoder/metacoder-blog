---
title: "Mit Spring den Classpath durchsuchen"
authors:
  - Felix Becker
date: 2012-12-07T15:43:10+02:00
lastmod: 2012-12-07T15:59:06+02:00
---

Für eine Plugin-Engine musste ich heute den Classpath nach Klassen durchsuchen, die von einer abstrakten Plugin-Klasse erben.  Von den gefundenen Klassen sollte aber keine Instanz erzeugt werden, eine Erweiterung des &lt;context:component-scan&gt; kam daher nicht in Frage. Die Lösung für das Problem ist die Klasse [ClassPathScanningCandidateComponentProvider](https://docs.spring.io/spring/docs/3.0.x/javadoc-api/org/springframework/context/annotation/ClassPathScanningCandidateComponentProvider.html):

```java
public class PluginRegistry {

    @SuppressWarnings("unchecked")
    public static Set<Class<? extends AbstractPlugin>> getAvailablePlugins() {

        ClassPathScanningCandidateComponentProvider componentProvider = new ClassPathScanningCandidateComponentProvider(false);
        componentProvider.addIncludeFilter(new AssignableTypeFilter(AbstractPlugin.class));

        Collection beanDefinitions = componentProvider.findCandidateComponents("my.plugins.package");

        Set<Class<? extends AbstractPlugin>> set = new HashSet<Class<? extends AbstractPlugin>>();

        for(BeanDefinition beanDefinition : beanDefinitions){
            try {
                set.add((Class<? extends AbstractPlugin>) Class.forName(beanDefinition.getBeanClassName()));
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
        }

        return set;
    }

}
```

Diese Lösung benötigt keinen Spring-Context. [Oliver Gierke](https://twitter.com/olivergierke) hat mir den entscheidenen Hinweis auf den [ClassPathScanningCandidateComponentProvider](https://docs.spring.io/spring/docs/3.0.x/javadoc-api/org/springframework/context/annotation/ClassPathScanningCandidateComponentProvider.html) gegeben, vielen Dank!
