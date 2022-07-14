---
title: Fail to execute "git add" on the Hexo project due to the theme submodule
date: 2022-06-09 02:25:24
tags:
  - Hexo
  - Git
categories:
  - Coding
    - Project
---

## Problem

I failed to ```git add .``` the source files of my Hexo blog project, and the error said:

{% codeblock line_number:false %}
You've added another git repository inside your current repository.
{% endcodeblock %}

## What's wrong

That was because inside this repository, I had cloned the theme files from another github repository.

So ```Git``` identifies the theme as a submodule.

## Solution

To solve this problem, we have to:

1. **Get to where the theme directory is, and remove the ```.git``` folder within it**

{% codeblock line_number:false %}
$ rm -rf .git
{% endcodeblock %}

We can check if it's removed successfully by listing out all the files within it:

{% codeblock line_number:false %}
$ ls -a
{% endcodeblock %}

2. **Remove cached files in root directory of our hexo project**

{% codeblock line_number:false %}
$ git rm -r --cached .
{% endcodeblock %}

3. **Add and commit all files again**

{% codeblock line_number:false %}
$ git commit -am 'commit message'
{% endcodeblock %}

<br/><br/><br/><br/>

**_References_**

[_Git is not tracking Hexo theme file_](https://stackoverflow.com/questions/53902804/git-is-not-tracking-hexo-theme-file)

[_git仓库包含子仓库时，add报错的解决办法_](https://cloud.tencent.com/developer/article/1583762)