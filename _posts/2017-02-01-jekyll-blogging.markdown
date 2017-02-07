---
layout: post
title: "GitHub Jekyll Blogging"
date: 2017-02-01 12:00:00
published: true
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

## Set up Jekyll Locally

Assuming this is all on a Mac. There are other links below that have Windows/Linux instructions, though at a high level the concepts are the same.

Start with the Command Line Toos suite:

`xcode-select --install`

Optionally, you can also install XCode

Get [Homebrew](http://brew.sh/)

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Get Ruby and Ruby Gems

`brew install ruby`

`gem install rubygems-update`

Finally, install **Jekyll**, and the Bundler tool

`gem install jekyll`

`gem install jekyll bundler`

## Running Jekyll

At this point the commands should match if you're on a Mac, PC, etc.. though I only tried on a mac. From here we will generate a demo site, run locally, and then you could copy the site to your demo GitHub repo push and see your demo site live on the web. 

In a terminal, *cd* to a working project folder or your GitHub repo if you want to deploy directly there.

Create a demo site

`jekyll new DemoSite`

*DemoSite* will be the folder your site is created in. If you want to create the site in your current folder path, skip the name, e.g.

`jekyll new .`

Now to run the site locally, in the path you created the site above, run:

`bundle exec jekyll serve --watch`

View the site at **localhost:4000**, *Control-C* to stop the site running.

Any file changes you make will be republished to the site automatically, **except for _config.yml**, that will require a restart of the service.

## Edit your Site

todo..

## Posts and Pages

## Alternative themes

Alternatively from spending the time custom authoring your own theme or playing with template imports, there is the school of thought to simply find a Jekyll site you like - or a base theme - and to fork that repository. Give credit where due on your site.

From here you can tweek and author your own posts using the theme you like.

## TravisCI

Todo..

Always be testing. 



# Helpful Links for Further Reading

[Building Static Sites with Jekyll and GitHub](http://programminghistorian.org/lessons/building-static-sites-with-jekyll-github-pages)

[Blogging Like a Dev](http://allandenot.com/development/2015/01/11/blogging-like-a-dev-jekyll-github-prose-io.html)

[How to set up your own R blog with Github pages and Jekyll Bootstrap](https://www.r-bloggers.com/how-to-set-up-your-own-r-blog-with-github-pages-and-jekyll-bootstrap/)

[GitHub Pages](https://pages.github.com/)

[Jekyll](https://jekyllrb.com/docs/home/)