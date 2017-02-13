---
title: Building Commonplace
layout: post
---

The first project in my attempt to build a project each month for the rest of 2016 took... a little over three months. Here's what I learned during the process of building [Commonplace](http://cmnplc.herokuapp.com).

## The Importance of Tooling
I've recently been bouncing back and forth between operating systems for development. For the past couple years, I've mainly developed on a 2011 iMac. I use Windows for my day job, though, so I decided earlier this year to try switching over to Windows for development as well. My goal for this project was to go all-in on using Microsoft's developer tools.

### Take One: Python, Django, and Visual Studio
My first choice for my technology stack was Python and Django built in Visual Studio and deployed to Azure. Microsoft has a [guide](https://azure.microsoft.com/en-us/documentation/articles/web-sites-python-ptvs-django-mysql) to getting a quick app up and running, so I figured that would be a good place to start. I was wrong.

First, the guide uses Python 2.7 and Django 1.7. Not a huge deal since those versions are still widely used, but for personal projects I try to be at least on the latest major version, if not on the bleeding edge. To make matters worse, Django V1.9 deprecated the `syncdb` command that was used in V1.7, so Python Tools for Visual Studio (PTVS) couldn't do everything I needed it to.

### Take Two: Node, Express, and Atom
After a few hours of struggling with Visual Studio, I decided to fall back on tools I'm more comfortable with rather than using something new just for the sake of doing things differently. Lesson learned: don't overcomplicate things by trying a bunch of new technologies all at once. I ended up using Node and Express as my framework, Atom as my text editor, Gulp for automation, and deploying to Heroku.

## The Importance of Database Design
I pretty quickly got to a point where I had a functioning single-user version of the app. Unfortunately, my initial database design didn’t translate well to multi-user authentication and authorization. Fixing my database design led to running over on my planned development timeline.

Because I'm a developer who has spent more time working on the front end than the back end (or with the full stack), this was an important reminder to me of the importance of design. Spending a bit more time up front to thoughtfully plan my database structure would have saved me hours in the long run.

## The Importance of Setting Expectations
The most important lesson I learned from this project had nothing to do with the actual development of the app. I realized setting lofty goals (such as developing a fully functioning app every month) is not always beneficial.

I took on several demanding projects at my day job during the course of developing Commonplace. Naturally, I had to prioritize the work that I actually get paid for, and so my commitment to Commonplace suffered.

Commonplace was a personal project, so having the timeline slip was no big deal. If it had been a client project, though, I would have been in trouble. Underestimating how long I would need to build the app would have led to long nights and unnecessary stress.
