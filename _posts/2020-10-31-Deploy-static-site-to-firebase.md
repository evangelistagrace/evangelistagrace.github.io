---
layout: post
title: "Deploy a static website to Firebase"
author: "Evangelista Grace"
categories: tutorials
tags: [Firebase]
image: 33.png
---

Ever wondered how you can get your website out of localhost and into the web so your friends can view it? Well as most of you may be aware, Firebase is also a hosting platform apart from being the popular NoSQL database. In this tutorial, we will be setting up hosting for a very simple (but pretty) static site.

[DEMO](https://dailydo-63793.web.app/)

Click [here](https://gist.github.com/evangelistagrace/fac8623f75cb1d3250acfe1e441889f4) to copy/download the source file for our simple landing page.

1\. Create project folder and copy the HTML file from the above link into this folder.

2\. Head to `console.firebase.google.com` and create a new project. Give your project a name and you can skip adding Google Analytics if you like.

3\. Once you have created a project, go to 'Project Overview' and click on 'Web' to add Firebase to our website.
{% include image.html url="/assets/img/deploy-to-firebase/1.png" %}

4\. Give your app a nickname and check 'Also set up Firebase Hosting for this app.'. Click 'Register app'.
{% include image.html url="/assets/img/deploy-to-firebase/2.png" %}

5\. Add the Firebase SDK before your `</body>` tag.
```
<script src="https://<your-web-app-name>.web.app/__/firebase/8.0.0/firebase-app.js"></script>
<script src="https://<your-web-app-name>.web.app/__/firebase/init.js"></script>
```
*remember to replace `your-web-app-name` with your app name.

6\. Run `npm install -g firebase-tools` in your project directory to install the Firebase CLI.

7\. Run `firebase login` and enter your login credentials.

8\. Run `firebase init` at the root of your project directory to initialize your firebase project.

{% include image.html url="/assets/img/deploy-to-firebase/3.png" %}

You will be prompted to select several options for your app. For this tutorial, we will only be needing hosting.

9\. For project setup, select 'Use an existing project' and select the project that you had created in step 2.

10\. Continue with the default setting for the rest of the setup. 

11\. After you have completed the Firebase initialization for your app, your project directory should look something like this:

{% include image.html url="/assets/img/deploy-to-firebase/4.png" %}

12\. Replace the `index.html` file inside 'public' folder with the pre-existing `index.html` file from step 1. Make any edits you wish to your website and once you're ready, run `firebase deploy`.

{% include image.html url="/assets/img/deploy-to-firebase/5.png" %}

Click on your web app link and voila you should be able to view your freshly deployed website!

{% include image.html url="/assets/img/deploy-to-firebase/6.png" %}
