---
layout: post
title: "How to deploy a local web app with database to Heroku"
author: "Evangelista Grace"
categories: tutorials
tags: [Heroku, PHP, PostgreSQL]
image: 32.png
---

I wish I had known how to deploy an app when I was doing my final year project. Deployment was such a mystery to me back then. I had the notion that there was so much to do and polish before an app gets deployed and that thought always kept me from giving it a try. 

For the longest time now, I've had plans to deploy the web application I had built for my final year project. You can now view it [here](https://budget-tracker-demo.herokuapp.com/src/). I realized just how easy deployment can be with Heroku. 

For this tutorial, I will be using a simple register/login web application that is connected to a local database. The source code can be downloaded from [here](https://github.com/evangelistagrace/Deploy-to-Heroku-with-Database-Tutorial). 

Some things to note before we get started:-
* Get your local development environment all setup. I will be using [WAPP](https://evangelistagrace.com/tutorials/WAPP-tutorial.html) (Windows, Apache, PHP, PostgreSQL). 
* Register for a free GitHub and Heroku account

Ready? Let's get started.

### App setup
1. Follow these [WAPP setup instructions](https://evangelistagrace.com/tutorials/WAPP-tutorial.html) if you haven't already got a local PHP development environment ready.
2. Download the source code or fork this [GitHub repository](https://github.com/evangelistagrace/Deploy-to-Heroku-with-Database-Tutorial).
3. Navigate to phpPgAdmin and create a database named `deploytoherokututorial`.
4. Go to 'tables' and import the SQL file from the source code.
5. Test your database connection by keying in `jane@mail.com`and `123` for the password in the login form.
![](/assets/img/deploying-a-web-app-with-db-to-heroku/1.png)
![](/assets/img/deploying-a-web-app-with-db-to-heroku/2.png)

### Publish app to GitHub
Now that you have a working application with a database, it's time get your app out there.

1. If you have downloaded the source code instead of forking the source repo, you will have to create a new repo.
2. Git init and push your application source code into the repo that you just created. Skip this and the previous step if you have forked from the source repo.


### Create a Heroku app and deploy GitHub branch to Heroku
1. Create a Heroku app.
![](/assets/img/deploying-a-web-app-with-db-to-heroku/4.png)
2. Under 'Deployment Method', choose 'GitHub' and connect your GitHub repo.
![](/assets/img/deploying-a-web-app-with-db-to-heroku/5.png)
3. Under 'Manual deploy', deploy the branch which contains the source code.
![](/assets/img/deploying-a-web-app-with-db-to-heroku/6.png)

Your application is now deployed. You won't be able to see anything if you try to view the app because it is trying to connect to your local database. We will have to provision a Heroku database and import local data into the provisioned database.

### Provision a Heroku database
1. Navigate to your app's 'Resources' section and search for 'Heroku Postgres' and provision it.
![](/assets/img/deploying-a-web-app-with-db-to-heroku/7.png)

### Import data from local database to Heroku database
1. Create a dump of your local database.
`pg_dump -U postgres -d deploytoherokututorial > C:\Users\Evan_\Documents\sql.sql --no-owner --no-acl`
2. Import your database backup to the Heroku database that you have provisioned.
`heroku pg:psql --app deploy-to-heroku-tutorial < C:\Users\Evan_\Documents\sql.sql`
Note: This command can only be run in cmd and not in Powershell.

You now have an online copy of your local database. Next is to connect this database to your app.

### Connect your Heroku database to your app
1. Navigate to your Heroku database settings and look for 'Database Credentials'.
![](/assets/img/deploying-a-web-app-with-db-to-heroku/8.png)
2. Copy your database credentials into your app's config file.
![](/assets/img/deploying-a-web-app-with-db-to-heroku/9.png)
3. Git push your config changes to your app repo.
3. Navigate back to your app dashboard and go to 'Deploy'. Under 'Manual Deploy', select 'Deploy Branch' to deploy the latest changes.

After refreshing, you should now be able to login with the username and password above and view a landing page.

That's it! You have successfully deployed an application with database to Heroku.




