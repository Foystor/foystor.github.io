---
title: Exception in thread "AWT-EventQueue-0" java.util.ConcurrentModificationException
date: 2022-06-02 20:01:00
tags:
  - Exception
  - Java
categories:
  - Coding
    - Project
---

When I used foreach-loop and lambda to apply a specific function to each element in a set data structure, the program threw the exception below:

{% codeblock line_number:false %}
Exception in thread "AWT-EventQueue-0" java.util.ConcurrentModificationException
{% endcodeblock %}

That was because:

> The for-each loop is used with both collections and arrays. It's intended to simplify the most common form of iteration, where the iterator or index is used solely for iteration, and not for any other kind of operation, such as removing or editing an item in the collection or array.

**We should avoid modifying elements in a set while iterating that set.**

* * *

There are **2** simple ways to fix that problem.

## Use the normal for loop

With the for (int i...) loop, we are not iterating the set, so we can modify the elements inside.

## Synchronize the operations properly

We can use a concurrent set whose iterators won't give the exception, such as ConcurrentSkipListSet.

In my case, I can change my codes:

```
getSensors().forEach(sensor -> changeSensorActivationStatus(sensor,false));
```

to the codes below:

```
ConcurrentSkipListSet<Sensor> sensors = new ConcurrentSkipListSet<>(getSensors());
sensors.forEach(sensor -> changeSensorActivationStatus(sensor,false));
```

<br/><br/><br/><br/>

**_Reference_**

_[Java ArrayList and Exception in thread "AWT-EventQueue-0" java.util.ConcurrentModificationException](https://stackoverflow.com/questions/17067626/java-arraylist-and-exception-in-thread-awt-eventqueue-0-java-util-concurrentm)_

_[How to modify a Collection while iterating using for-each loop without ConcurrentModificationException?](https://stackoverflow.com/questions/6958478/how-to-modify-a-collection-while-iterating-using-for-each-loop-without-concurren)_

_[Threads in Java. "AWT-EventQueue-0" java.util.ConcurrentModificationException](https://stackoverflow.com/questions/18397075/threads-in-java-awt-eventqueue-0-java-util-concurrentmodificationexception)_