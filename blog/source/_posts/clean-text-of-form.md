---
title: Clean the text of the form after submission
date: 2022-07-06 11:47:23
tags:
  - Java
  - Spring Boot
  - Spring MVC
categories:
  - Coding
    - Project
---

## Problem

I developed a chat application with ```Spring MVC```, where I can submit messages along with my username, and those messages will be visible to any user that navigates to the ```/chat``` URL.

But when I submit my message, the message is not removed from the Message Text input so I have to remove it manually.

<figure>
  <img src=remain.png>
</figure>

And here is the code in ```ChatController``` Class:

<figure>
  <img src=controller.png>
  <figcaption>ChatController</figcaption>
</figure>

## What's wrong

when we initialize our application, ```Spring``` will scan our project and instantiate only one ```ChatForm``` object and it will include it in the ```Application Context```, so every time we access the ```chat.html``` page, the same instance of the ```ChatForm``` will be passed and will be used to transport the form data present in the ```chat.html``` page to the ```addChat``` method.

This reuse of the same instance of the ```ChatForm``` is done precisely so that unnecessary instances of it are created and thus end up increasing the memory consumption of our application and affect its performance.

## Solution

Add controller code that sets the Message Text of the ```ChatForm``` object to ```""```;

<figure>
  <img src=clean-text.png>
</figure>

<br/><br/><br/><br/>

**_References_**

[_Why do we need chatForm.setMessageText("") in the controller_](https://knowledge.udacity.com/questions/625850)