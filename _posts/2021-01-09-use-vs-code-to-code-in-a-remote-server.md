---
layout: post
title: "Using VS Code to code in a remote server"
author: "Evangelista Grace"
categories: tutorials
tags: [VS Code, SSH]
image: 35.png
---

In my current workplace, we deal with a lot of remote servers. From networking, to configuring tools, programming and deploying apps. The remote scene has taken center stage especially with the extended work from home orders brought about by the plight of Covid. In my current role, my tasks demands heavy usage of the command line and often heavy file write operations in the company's remote servers. Using Vim to write programs with multiple indentations easily made it into my list of dreads. Naturally, I seeked to mitigate this cumbersomeness of my day to day work.

VS Code has always been my go-to code editor simply because of its ease of use, customization, and versatility. For this reason, I seeked to know if by any chance I could use it in a remote server. It didn't take me long to find out that that was indeed possible!

The setup is simple and straight-forward. Let's jump right in.

### Prerequisites

Whether you're on a Windows or Linux machine, you need to first __ensure that you can work with SSH commands__ from the terminal. If you don't already have a SSH client installed, installing OpenSSH in your system is a good way to get started.

You would also need a __private and public key pair__ from the remote server. This is usually administered by the admin of the remote server. If you're running your own instance of a remote server, then simply generate an SSH key pair from your local machine using ssh-keygen and add your public key to the remote server.

Lastly, you would need __VS Code installed__ in your local machine!

### Step 1: Edit the SSH config file

One way to easily SSH into a remote host is by editing your SSH config file to include the hostname and your user credentials of the remote host:

```
Host <any name you like>
    HostName <remote host name/IP address>
    User <your username recognized by the remote host>
    IdentityFile <location of your SSH key>
```

{% include image.html url="/assets/img/using-vs-code-in-remote-server/1.png" %}

In the sample configuration above, I am able to SSH into the remote host node01 simply by running `ssh node01` instead of `ssh XL204329@10.55.4.237`.

### Step 2: Install remote-ssh extension

In VS Code, search for 'remote-ssh' extension and install the extension by Microsoft.

{% include image.html url="/assets/img/using-vs-code-in-remote-server/2.png" %}

### Step 3: Connect to the remote host

You should now see an option to 'Open a remote window' on the bottom left of the editor.

{% include image.html url="/assets/img/using-vs-code-in-remote-server/3.png" %}

Click the option and choose 'Remote-ssh: Connect to host...'

{% include image.html url="/assets/img/using-vs-code-in-remote-server/4.png" %}

Now you should see a list of host(s) that you have configured in step 1. Click to connect to a host. Here, I will connect to 'node01'.

{% include image.html url="/assets/img/using-vs-code-in-remote-server/5.png" %}

You should now see a new window with the name of the remote host you just connected to.

{% include image.html url="/assets/img/using-vs-code-in-remote-server/6.png" %}

### Code away!

You can now freely browse your folder and files in the remote server and code as though you're coding locally!

{% include image.html url="/assets/img/using-vs-code-in-remote-server/7.png" %}

Happy coding!

Reference(s):
- [https://code.visualstudio.com/blogs/2019/07/25/remote-ssh](https://code.visualstudio.com/blogs/2019/07/25/remote-ssh)