---
title: Dynamic Proxy and UndeclaredThrowableException
tags:
  - Java
categories:
  - Coding
    - Study
date: 2022-03-24 19:21:34
---

In an exercise about Dynamic Proxy, I implemented a dynamic proxy that implements the Client interface by looking in the map for the name properties.

The underlying delegate object is a map, which is different from the interface that the proxy object implements (So we could say that this dynamic proxy acts as an **adapter**).
<!-- more -->

* * *

## Code Explaining

Here is a code snippet from the custom InvocationHandler:

```
if (method.getDeclaringClass().equals(Object.class)) {
    // Invoke toString() and hashCode() directly on the property Map.
    try {
      return method.invoke(properties, args);
    } catch (InvocationTargetException e) {
      throw e.getTargetException();
    } catch (IllegalAccessException e) {
      throw new RuntimeException(e);
    }
}
```

All Java objects inherit methods from Object class like toString, equals and hashCode.

In this exercise, we are not concerned about these methods. We want to intercept only get methods defined in the Client interface.

So we check whether a method is declared in the Object class, and just invoke the method with the original arguments. This is like ignoring the proxy.

But we should also take care if these methods (toString, equals, hashCode) throw an exception.

Notice that method.invoke() would rethrow that exception wrapped in an **InvocationTargetException**.

We don't want to change the behavior of these methods, so we should unwrap the exception and rethrow it exactly as the method call will do if no proxy exists:

```
throw e.getTargetException();
```

But there is a special case where a **[UndeclaredThrowableException](https://www.baeldung.com/java-undeclaredthrowableexception#java-dynamic-proxy)** may be thrown and we should treat it also.

<br/><br/><br/><br/>

**_Reference_**

_[When Does Java Throw UndeclaredThrowableException?](https://www.baeldung.com/java-undeclaredthrowableexception#java-dynamic-proxy)_

[_jdk动态代理异常处理分析， UndeclaredThrowableException_](https://segmentfault.com/a/1190000012262244)