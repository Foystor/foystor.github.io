---
title: Set up AWS EC2 Parse Server in Android
tags:
  - Android
  - AWS
  - Parse
categories:
  - Coding
    - Project
date: 2022-03-15 18:09:32
---

I learned Android along with [The Complete Android Oreo Developer Course](https://www.udemy.com/course/the-complete-android-oreo-developer-course/) and was stuck in the Instagram Clone chapter when I tried to set up the AWS EC2 Parse Server.

After googling many times, I finally figured it out and got access to the Parse Dashboard!

If you also have trouble setting up the Parse Server, just go ahead and see if my solutions would inspire you.
<!-- more -->

## Connect to the instance using SSH client

In Amazon Dashboard choose ```Instances``` from the left sidebar, and then select the instance we would like to connect to.

<figure>
  <img src=connect.png>
  <figcaption>Select an instance and click the Connect button</figcaption>
</figure>

<figure>
  <img src=ssh-client.png>
  <figcaption>Click on the SSH client tab</figcaption>
</figure>

We can just follow the instructions above, but there might be some unexpected problems.

So refer to the steps below:

1.  **Open the terminal and locate the private key file**

Check the ```.pem.txt``` file we've just downloaded when creating a new key pair.

Remove the **".txt"** behind to keep it as a ```.pem``` file. This ```.pem``` file is our private key file.

Then open up the terminal and get to where the file is. In my case it's on the desktop:

{% codeblock line_number:false %}
$ cd Desktop
{% endcodeblock %}

1.  **Change the permissions of the ```.pem``` file so only the root user can read it**

{% codeblock line_number:false %}
$ chmod 400 instagramandroid.pem
{% endcodeblock %}

3.  **Connect to the instance using its Public DNS**

In this step, simply copying the command above might get errors.

We should verify that we are connecting with the [appropriate user name for our AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html#TroubleshootingInstancesConnectingPuTTY).

In my case:

{% codeblock line_number:false %}
$ ssh -i "instagramandroid.pem" bitnami@ec2-3-14-27-1.us-east-2.compute.amazonaws.com
{% endcodeblock %}

If we see something on our terminal screen like this:

<figure>
  <img src=success-connect.png>
</figure>

That means we've successfully connected to our instance.

## Do some setup

Once we get to that screen, we need to move into where the parse code is, to do some setup:

{% codeblock line_number:false %}
$ cd /opt/bitnami/parse
{% endcodeblock %}

Next, we have a file that we need to edit:

{% codeblock line_number:false %}
$ vi config.json
{% endcodeblock %}

Inside the file, we can see the information of the server:

<figure>
  <img src=config.png>
</figure>

We can paste the information required to initialize the Parse Server in our android application.

## Connect to the Parse Dashboard

We should be able to visit the Parse Dashboard by surfing to our server public IP address (which we can get from the AWS console) in our browser.

<figure>
  <img src=url.png>
</figure>

<figure>
  <img src=login.png>
  <figcaption>login page for the Parse Server</figcaption>
</figure>

Get the user name and password in the terminal:

{% codeblock line_number:false %}
$ cat bitnami_credentials
{% endcodeblock %}

<figure>
  <img src=dashboard.png>
  <figcaption>Log in successfully</figcaption>
</figure>

## Configure publicServerURL

Let's say we upload a photo to the parse server.

When we click on the image link, it opens up a url using the localhost IP again.

<figure>
  <img src=link.png>
</figure>

<figure>
  <img src=localhost.png>
</figure>

That's a promblem. Because it means we can not get access to this photo in our program.

{% blockquote Parse's documentation %}
When using files on Parse, you will need to use the publicServerURL option in your Parse Server config. This is the URL that files will be accessed from, so it should be a URL that resolves to your Parse Server. Make sure to include your mount point in this URL.
{% endblockquote %}

The ```publicServerURL``` is explicitely required for those cases. Parse server injects the ```publicServerURL``` for all our files at runtime.

Every file URL is replaced at runtime, with the full URL. If we set the ```publicServerURL```, this will be set in all the responses.

All we have to do is adding the ```publicServerURL``` parameter to the server.

1. **Open the ```config.json``` file**

{% codeblock line_number:false %}
$ cd /opt/bitnami/parse
{% endcodeblock %}

{% codeblock line_number:false %}
$ vi config.json
{% endcodeblock %}

2. **Add a new line below the ```serverURL``` parameter**

{% codeblock line_number:false %}
"publicServerURL": "http://IP/parse"
{% endcodeblock %}

3. **Save it and restart Parse running**


{% codeblock line_number:false %}
$ sudo /opt/bitnami/ctlscript.sh restart parse
{% endcodeblock %}

Now we can refresh the Parse Dashboard page, and can get to the right URL to download photos successfully.

<br/><br/><br/><br/>

**_References_**

[_Unable to find /opt/bitnami/apps/parse/htdocs/server.js_](https://community.bitnami.com/t/unable-to-find-opt-bitnami-apps-parse-htdocs-server-js/78200)

[_Error: Server refused our key or No supported authentication methods available_](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html#TroubleshootingInstancesConnectingPuTTY)

[_I cannot or do not know how to connect to the parse dashboard_](https://community.bitnami.com/t/i-cannot-or-do-not-know-how-to-connect-to-the-parse-dashboard/97505)

[_Can't find password in AWS System Log for new AMI created from Bitnami Wordpress AMI_](https://stackoverflow.com/questions/38648071/cant-find-password-in-aws-system-log-for-new-ami-created-from-bitnami-wordpress)

[_Parse Server - Image files' path returns localhost_](https://github.com/parse-community/parse-server/issues/4290)

[_Unable to download or view images_](https://community.bitnami.com/t/unable-to-download-or-view-images-i-o-error/71400)