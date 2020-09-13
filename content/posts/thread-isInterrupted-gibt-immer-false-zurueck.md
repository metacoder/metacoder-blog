---
title: "Thread.isInterrupted() gibt immer false zurück"
authors:
  - Felix Becker
date: 2012-12-07T07:23:20+02:00
lastmod: 2012-12-07T07:46:03+02:00
---

Ich bin beim Implementieren von Lasttests über einen sehr fiesen Java-Bug im Multithreadingbereich gestoßen:

```java
Thread t = new Thread(new Runnable(){

    @Override
    public void run() {
        while(Thread.currentThread().isInterrupted()){
            System.out.println("running into take");
            try {
                System.out.println("received: " + blockingQueue.take());
            } catch (InterruptedException e) {
                System.out.println("caught interrupted exception.");
            }
        }
    }

});

t.start();

blockingQueue.add("foo");

t.interrupt();
```

Bei diesem Code hätte ich erwartet, dass das t.interrupt() dafür sorgt, dass ein blockendes blockingQueue.take() in den catch-Block springt und danach der Thread terminiert, da Thread.currentThread().isInterrupted() true zurückgibt. Bei mir springt, bei installiertem Java 1.6 32 / 64 Bit,  der Thread nach dem catch sofort wieder zurück zu blockingQueue.take() - **weil Thread.currentThread.isInterrupted() immer false zurückgibt**.

Über Stackoverflow bin ich auf folgenden JVM-Bug gestoßen:

http://bugs.sun.com/view_bug.do?bug_id=6772683

Um einen Thread sicher über ein Thread.interrupt() zu beenden und dabei den Bug zu umgehen verwendet man am besten eine Shutdown-Flag:

```java
Thread t = new Thread(new Runnable(){

    private boolean shutdown = false;

    @Override
    public void run() {
        while(!shutdown){
            System.out.println("running into take");
            try {
                System.out.println("received: " + blockingQueue.take());
            } catch (InterruptedException e) {
                System.out.println("caught interrupted exception.");
                shutdown = true;
            }
        }
    }

});

t.start();

blockingQueue.add("foo");

t.interrupt();
```

Damit wird der Thread sauber beendet. Warum der Bug im aktuellen JDK 1.6 immer noch nicht behoben ist, ist mir ein Rätsel. Wie es mit Java 7 / Windows aussieht konnte ich noch nicht testen.
