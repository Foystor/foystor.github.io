---
title: Intellij Cannot Resolve Symbols In Spring Starter Project
date: 2022-07-02 00:15:50
tags:
  - Java
  - Maven
  - Spring Boot
categories:
  - Coding
    - Project
---

## Problem

I downloaded a Spring starter project from [Spring Initializr](https://start.spring.io/), but when I opened the project in Intellij, I got ```cannot reslove symbol``` errors:

<figure>
  <img src=error.png>
</figure>

## Try to fix

I googled and looked through StackOverflow, and I tried the solutions below:

1. Invalidate caches and restart
2. ```mvn clean install```
3. Delete the ```.m2``` directory, and then ```mvn clean install```
4. ```mvn clean install -U```

**But nothing changes.**

I tried to download more Spring boot starter projects for experiment. Then when I first opened one of those projects, I got the error below:

{% codeblock line_number:false %}
org.codehaus.plexus.component.repository.exception.ComponentLookupException: com.google.inject.ProvisionException: Unable to provision, see the following errors:

1) Error injecting constructor, java.lang.NoSuchMethodError: org.apache.maven.model.validation.DefaultModelValidator: method <init>()V not found
  at org.jetbrains.idea.maven.server.embedder.CustomModelValidator.<init>(Unknown Source)
  while locating org.jetbrains.idea.maven.server.embedder.CustomModelValidator
  at ClassRealm[maven.ext, parent: ClassRealm[plexus.core, parent: null]] (via modules: org.eclipse.sisu.wire.WireModule -> org.eclipse.sisu.plexus.PlexusBindingModule)
  while locating org.apache.maven.model.validation.ModelValidator annotated with @com.google.inject.name.Named(value=ide)

1 error
      role: org.apache.maven.model.validation.ModelValidator
  roleHint: ide
{% endcodeblock %}

I searched for solutions to this error. Then I saw someone also got this error in the same version (**2021.3.2**) of ```Intellij``` as mine, and the problem was solved by downgrading the version of ```maven``` from **3.8.5** to **3.6.3**.

## What's wrong

So FINALLY, the ```cannot reslove symbol``` problem was due to an incompatibility between the ```Maven``` and ```IntelliJ``` versions.

**The release of the Maven version should be earlier than the one of Intellij.**

## Solution

I was using ```Maven``` **3.8.5**, so I upgraded my ```Intellij``` from **2021.3.2** to **2022.1.3** (Community Edition), then everything works fine.

<br/><br/><br/><br/>

**_References_**

[_2021.3.2版本idea配置maven出现org.codehaus.plexus.component.repository.exception.ComponentLookupException_](https://blog.csdn.net/qq_41563868/article/details/123527235)

[_IDEA版本与MAVEN版本对应关系，及历史MAVEN版本下载_](https://www.cnblogs.com/hpwlym/p/15353847.html)