---
layout: post
title: "Creating Subdomains for Github Pages Hosted with Namecheap"
author: "Evangelista Grace"
categories: tutorials
tags: [GitHub Pages, Namecheap]
image: 29.png
---

For starters, a subdomain is basically an extension of your base domain. If you're using GitHub Pages to host your website, the default domain that you get is `<username>.github.io`. If you want a custom domain, sites like Namecheap, GoDaddy, HostGator and many more of the like offer a wide variety of domain names that are up for grabs. An example subdomain for a default Github Pages domain would be `blog.<username>.github.io`.

I purchased my website's domain a while back. While this was already a blog-like website, I had intended to separate tech and non-tech related posts. I wanted a `blog` subdomain to redirect to my non-tech related posts (because I'm a cheapskate and I don't intend on buying another domain just for this :p). You can view my non-tech blog at [blog.evangelistagrace.me](https://blog.evangelistagrace.me/). When I was looking for a tutorial to follow, I found that there was a lack of tutorial online for creating a subdomain for GitHub Pages hosted with Namecheap. So here's a tutorial for just that.

Note: If you intend to create a subdomain for GitHub Pages default domain, follow [step 1](#add-custom-domain) only. If you want to create a subdomain for an existing custom domain, follow both [step 1](#add-custom-domain) and [step 2](#add-cname-record).

#### <a name="add-custom-domain">Add a custom domain to a Github repo</a>
<hr>

I created a new repo and moved all my desired posts there. You can name your repo whatever you want. What's important to note is that your desired repo must have a `index.html` file in either your 'master' branch or a 'gh-pages' branch, depending on which branch you wish to add the subdomain to. 

![](/assets/img/subdomain-github-pages-namecheap/1.png)

Once you have your repo ready, click on the repo's settings and add your desired subdomain under 'Custom Domain'.

![](/assets/img/subdomain-github-pages-namecheap/2.png)

Click 'Save' and this should have created a CNAME file in your repo. Optionally, tick 'Enforce HTTPS' to get the nice green lock beside your subdomain. This will take some time to show.

#### <a name="add-cname-record">Add a CNAME record in Namecheap</a>
<hr>

Log in to your Namecheap account and navigate to your domain list and click on 'Manage > Advanced DNS > Add new record'.

For 'Type' Choose 'CNAME record' from the dropdown. For the 'Host', enter just the first part of your subdomain (e.g: blog). As for 'Value', enter your original GitHub Pages domain, i.e. `<username>.github.io`. Set the 'TTL' to 'Automatic' and save the record.

![](/assets/img/subdomain-github-pages-namecheap/3.png)

And that's it! Head to your new subdomain and don't fret if you don't see your webpage yet. Wait a little bit longer and check back in. Your new subdomain should be up and runningin no time.
