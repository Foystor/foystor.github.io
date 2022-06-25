---
title: "\"git add .\" failed for hexo project due to the theme submodule"
date: 2022-06-09 02:25:24
tags:
  - Hexo
  - Git
categories:
  - Coding
    - Project
---

I wanted to backup the source files of my blog, but I failed to "git add ." my hexo project.

It said:

{% codeblock line_number:false %}
You've added another git repository inside your current repository.
{% endcodeblock %}

That was because inside this repository, I had cloned the theme files from another github repository.

So git identifies the theme as a submodule.

* * *

To solve this problem, we have to:

1. **Get to where the theme directory is, and remove the .git folder within it**

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

**_Reference_**

[_Git is not tracking Hexo theme file_](https://stackoverflow.com/questions/53902804/git-is-not-tracking-hexo-theme-file)

[_git仓库包含子仓库时，add报错的解决办法_](https://cloud.tencent.com/developer/article/1583762)