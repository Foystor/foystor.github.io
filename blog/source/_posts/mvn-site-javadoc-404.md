---
title: "\"mvn site\" with maven-javadoc-plugin can't access to Javadoc (404)"
tags:
  - Java
  - Maven
categories:
  - Project
date: 2022-05-02 21:04:57
---

## Problem

I built a project using Maven Quickstart Archetype.

Then I followed the usage guide of the Apache Maven Javadoc Plugin, and expected to have a Javadoc included in my project documentation.

<figure>
  <img src=usage.png>
  <figcaption>Apache Maven Javadoc Plugin Usage</figcaption>
</figure>

<!-- more -->
<br/>

<figure>
  <img src=copy.png>
</figure>

<br/>

However, I got a 404 on my Javadoc page.

<figure>
  <img src=404.png>
</figure>

* * *

## Solution

Change the version of maven-javadoc-plugin to **3.2.0** to solve this problem.

<figure>
  <img src=edit.png>
</figure>

<br/>

Compared to the target directory generated when executing "mvn site" on the **3.4.0** version, the one generated on the **3.2.0** version added files as following:

<figure>
  <img src=compare.png>
</figure>