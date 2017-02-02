---
layout: post
title: "GitHub Jekyll Blogging"
date: 2017-02-01 12:00:00
published: false
categories: posts
tags:  jekyll blog github
---

# How to GitHub Jekyll Blog

Part of this blog inspiration was to get more into GitHub pages and using GitHub as a host. This brings about a static-site design, Jekyll being GitHub's choice of static-site generator. 

## What is GitHub Pages?

Simply they are small websites can be hosted from public repositories on GitHub. 

GitHub Pages is designed to host your personal, organization, or project pages directly from a GitHub repository. 

A static site hosting service doesn't support server-side code such as, PHP, Ruby, or Python. Simply compose markdown then either on your local machine or have GitHub "build" the site into html/css. 

Note everything going into your GitHub Pages will be **public**, do not publish any private/sensitive information.

## Getting started

Log into your GitHub account. Go to repositories and click on **New Repository**

Give your repository a name. This will become the URL of your "blog" on GitHub, assuming we don't utilize another DNS name and CNAME configuration. We will come back to that later. The name as a URL will appear as *https://{your_account}.github.io/{repo_name}*

Feel free to give the repository a description. Select Public. Private will be a different discussion. 

Initalize with a .gitignore, recommend starting with the Jekyll option. Choose a license, **MIT License** was my choice as I wanted to alignt with the theme I selected later on.

Then **Create repository**

Once on the page for your newly created repository, on the top menu select **Settings**. Scroll down to **GitHub Pages** and select a branch to publish from. If this repository is only used for GitHub Pages / a blog, *master* would be fine. If this is a project repository with other non-GitHub Pages work, highly recommend using a branch *gh-pages* or the */docs* folder on *master* option.

At this point you can select GitHub published theme if you choose. Theme's will sort of dictate the style of site you create. The GitHub themes are ideal for flat sites, like the front page of a project or organization. Not ideal for a blog. Though the code is all at your disposal to custimize to whatever use you have. 

Finally **clone** the repository to your machine and being building your site.

## TravisCI





# Helpful Links for Further Reading

[Building Static Sites with Jekyll and GitHub](http://programminghistorian.org/lessons/building-static-sites-with-jekyll-github-pages)

[Blogging Like a Dev](http://allandenot.com/development/2015/01/11/blogging-like-a-dev-jekyll-github-prose-io.html)

[How to set up your own R blog with Github pages and Jekyll Bootstrap](https://www.r-bloggers.com/how-to-set-up-your-own-r-blog-with-github-pages-and-jekyll-bootstrap/)

[GitHub Pages](https://pages.github.com/)

[Jeykll](https://jekyllrb.com/docs/home/)