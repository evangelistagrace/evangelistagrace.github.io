---
layout: post
title: "Deploying a Laravel app to Heroku (with Heroku Postgres)"
author: "Evangelista Grace"
categories: tutorials
tags: [GitHub, Heroku, Laravel, PostgreSQL]
image: 28.png
---

There are many ways to deploy an app to Heroku, depending on the type of application. Here is an easy way I found to deploy a Laravel app to Heroku with Heroku Postgres provisioned as the database.

Contents:
1. [Prerequisites](#prerequisites)
2. [Create a new Laravel app](#create-new-app)
3. [Deploy application to Heroku](#deploy-app)
4. [Set config variables for the application](#config-vars) 
5. [Provisioning Heroku Postgres database](#provision-database)
6. [Extras](#extras)

#### <a name="prerequisites">Prerequisites</a>
<hr>
Ensure that you've installed the following:
- Composer (dependency manager)
- Git client
- A Heroku account 
- Postgres (optional: needed if making changes to local database)

The Composer version used in this tutorial is v1.10.1. Laravel's latest version, Laravel 6, was used at the time of writing. A Heroku free account can be used for this tutorial.

#### <a name="create-new-app">Create a new Laravel app</a>
<hr>
Run the following commands in your command line to create and run a new Laravel application called `myapp`.

1\. `composer create-project --prefer-dist laravel/laravel myapp`

2\. `cd myapp`

3\. `php artisan serve`

You should now see a preview of your laravel application in 127.0.0.1:8000.

{% include image.html url="/assets/img/deploying-a-laravel-app-to-heroku/1.jpg" %}

#### <a name="deploy-app">Deploy application to Heroku</a>
<hr>

4\. Login to Heroku by running `heroku login`

5\. Create a Procfile in your project's root directory, i.e. /myapp/. Paste the the following line into the Procfile.
    <br>
    `web: vendor/bin/heroku-php-apache2 public`
    ![](/assets/img/deploying-a-laravel-app-to-heroku/2.jpg)

6\. Initialize a local git repository. Run `git init` in your terminal.

7\. Create a new app in Heroku by running `heroku create`. This will initialize a git remote and generate a random name for your project. In this tutorial, the project name generated was [guarded-meadow-03537](guarded-meadow-03537.herokuapp.com). More info on changing the project name in the **Extras** section below.

8\. Commit your code by entering the following commands:
 ```
git add .
git commit -m "initial commit"
git push heroku master
```
Enter the commands above each time you wish to deploy any changes to your source code.

Hold your horses, we're nearly there. 

#### <a name="config-vars">Set config variables for the application</a>
<hr>

9\. In your Heroku dashboard, navigate to your application's settings, scroll down to 'Config Vars' and click on 'Reveal Config Vars'. You will see key and value blank fields.
![](/assets/img/deploying-a-laravel-app-to-heroku/3.jpg)

10\. Now head back to your project files in your code editor and view the '.env' file located in the project's root directory. Copy the key and value pairs for 'APP_' into the fields found in step 9.
![](/assets/img/deploying-a-laravel-app-to-heroku/4.jpg)
![](/assets/img/deploying-a-laravel-app-to-heroku/5.jpg)

11\. Run `heroku open`. You should see your app up and running with its new URL.
 ![](/assets/img/deploying-a-laravel-app-to-heroku/6.jpg)


#### <a name="provision-database">Provisioning Heroku Postgres database</a>
<hr>
12\. In your Heroku dashboard, navigate to your application's resources and search for 'Heroku Postgres' under 'Add-ons'.

13\. Choose a plan that you like. For this tutorial, we will use a free 'Hobby Dev' plan.

14\. Find out your database credentials by running `heroku pg:credentials:url`
 ![](/assets/img/deploying-a-laravel-app-to-heroku/7.jpg)

15\. Update the 'DB_' key values in your '.env' file according to the values found in the previous step.
 ![](/assets/img/deploying-a-laravel-app-to-heroku/8.jpg)

16\. Repeat steps 9-10 except this time for the 'DB_' key and value pairs.

17\. Run the following commands to authenticate and create tables in the database:
```
heroku run php artisan migrate
composer require laravel/ui
php artisan ui vue --auth
php artisan migrate
```

18\. Follow step 8 to commit and deploy changes.

19\. Run `heroku open`. You should now see 'Login' and 'Registration' links in the application.
 ![](/assets/img/deploying-a-laravel-app-to-heroku/9.jpg)

20\. Test the login and registration functions in step 19 to see if the database is setup properly.

Congratulations! You have successfully deployed a Laravel application to Heroku with a Heroku Postgres database.

#### <a name="extras">Extras</a>
<hr>

##### 👉Changing the name of your application
21\. In your Heroku dashboard and navigate to your application's settings. Under app information, edit your application name. Click 'Save'.

22\. Run the following commands to update your git remote.
```
git remote rm heroku
heroku git:remote -a newname
```

##### 👉Pulling data from remote database to local database
23\. To ensure that you're logged in with the correct credentials, set your postgres username and password in the terminal.
```
set PGUSER=postgres
set PGPASSWORD=123456
```

24\. Run this command to pull data from you remote database into a local database `heroku pg:pull DATABASE_URL mylocaldb --app mylaravelwebsite`. Make sure that no existing local database has the same name as the new local database that you are creating. Here, the local database is named 'mylocaldb' and the app name is 'mylaravelwebsite'.

25\. Open pgAdmin and login to your postgres account. Check for the new database that was created and the tables in it. Query the 'users' table to view the records of registered users of your application.

##### 👉Pushing data from local database to remote database
26\. Repeat step 23.

27\. Run this command to push data from you local database into a remote database `heroku pg:push mylocaldb DATABASE_URL --app mylaravelwebsite`. Ensure that your remote database is empty before executing this commad. You will be prompted to reset your remote database if it is not empty.

<div style="float:right; font-style: italic; font-family: Georgia">-Evan</div>