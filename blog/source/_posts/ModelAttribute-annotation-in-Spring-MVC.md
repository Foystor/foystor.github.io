---
title: Using @ModelAttribute annotation in Spring MVC
date: 2022-07-09 04:43:59
tags:
  - Java
  - Spring Boot
  - Spring MVC
categories:
  - Coding
    - Project
---

## Background

I developed a chat application with ```Spring MVC```, and here are the codes in the ```ChatController``` Class and the ```chat.html``` page:

<figure>
  <img src=controller.png>
  <figcaption>ChatController</figcaption>
</figure>

<figure>
  <img src=html.png>
  <figcaption>chat.html</figcaption>
</figure>

## Problem

Notice that I used the ```newChatForm``` identifier in ```th:object``` present in the ```chat.html``` page to name the ```ChatForm``` parameter both in the ```getChatPage``` and ```addChat``` method.

But when I removed the ```@ModelAttribute("newChatForm")``` in one of those methods, I got the error below:

<figure>
  <img src=error.png>
</figure>

## What's wrong

Since I included ```th:object``` in the ```chat.html``` page, and so for the mapping to be done, a ```ChatForm``` object needs to be included in the ```Spring Container``` to be used by ```Spring MVC``` to map the form data.
<br/>```Spring MVC``` will check in the ```Spring Container``` if there is any instance with the name similar to ```th:object```. If there is one it ends up using that instance in the mapping.

But when I only put ```ChatForm newChatForm``` in the method parameter, I am telling ```Spring``` that this parameter must be instantiated (with the default name **```chatForm```**, which is the camelCase pattern of ```ChatForm```), because it will be used in the method as well as the ```Model```. The ```ChatForm``` is only included in the ```Spring Container``` not being included by the ```Model``` in the ```chat.html``` page.

Since the default name ```chatForm``` is different from the ```newChatForm``` identifier in ```th:object``` present in the ```chat.html```, ```Spring MVC``` can't find any instance to map the form data, which causes the error I received.

However, when I use ```@ModelAttribute("newChatForm")```, I am indicating to ```Spring``` that besides instantiating the ```ChatForm```, this ```ChatForm``` must be included in the page through the ```newChatForm``` identifier, since this inclusion is made through the ```Model```. So no error is thrown.

**So, the inclusion of the ```@ModelAttribute``` depends on the identifiers being used**, if both are equal, its inclusion is not necessary, otherwise, we need it so that a mapping error does not occur.

## Solution

1. **Use ```chatForm``` identifier both in ```chat.html``` and in the ```ChatController``` method parameter**

```html chat.html
<form th:action="@{'/chat'}" th:object="${chatForm}" action="#" method="POST">
  // original code
</form>
```

```java ChatController.java
@GetMapping
public String getChatPage(ChatForm chatForm, Model model) {
    // original code
}

@PostMapping
public String addChat(ChatForm chatForm, Model model) {
    // change variable name from "newChatForm" to "chatForm"
}
```

This way ```Spring MVC``` directly uses the ```ChatForm``` instance in the ```Spring Container``` for mapping, so we don't need to use ```@ModelAttribute```.

However, this approach requires that other developers have some knowledge of how ```Spring``` works under the hood, otherwise they will probably delete the ```ChatForm``` thinking that this parameter is not doing anything.

2. **Create the ```getChatForm``` method to return the instance**

Every time the ```"/chat"``` endpoint is triggered, ```Spring``` will automatically assign a ```ChatForm``` instance to the ```chat.html``` page.

For improving the readability of the code by showing more clearly the process of assigning the instance by ```Spring```, 

We can create the ```getChatForm``` method which will be responsible for including the object in the ```Spring Container``` and which can be used by ```Spring MVC``` for mapping:

```java ChatController.java
@ModelAttribute("newChatForm")
public ChatForm getChatForm {
    return new ChatForm();
}
```

This way we can remove the ```@ModelAttribute("newChatForm")``` from both methods and the ```ChatForm``` from the ```getChatPage``` method:

```java ChatController.java
@GetMapping
public String getChatPage(Model model) {
    // original code
}

@PostMapping
public String addChat(ChatForm newChatForm, Model model) {
    // original code
}
```

<br/><br/><br/><br/>

**_References_**

[_Difference between using and not using @modelAttribute in method parameter_](https://stackoverflow.com/questions/3502749/difference-between-using-and-not-using-modelattribute-in-method-parameter)

[_What is the meaning of @ModelAttribute annotation at method argument level?_](https://stackoverflow.com/questions/4700804/what-is-the-meaning-of-modelattribute-annotation-at-method-argument-level)

[_Using @ModelAttribute annotation in Spring MVC_](http://opensourceforgeeks.blogspot.com/2016/01/using-modelattribute-annotation-in.html)

[_Remove @ModelAttribute("newChatForm") and get error_](https://knowledge.udacity.com/questions/868688)

[_Why we should add @ModelAttribute in GET method?_](https://knowledge.udacity.com/questions/731386)