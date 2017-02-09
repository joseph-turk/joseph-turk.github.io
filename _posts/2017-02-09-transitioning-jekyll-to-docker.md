---
title: Transitioning Jekyll to Docker
layout: post
---

For the past couple years, I've been maintaining this Jekyll site with the full development stack (small though it is) running natively on my local machine. Recently, I decided to fully embrace containerization with Docker. Here are my experiences moving this site to a Docker environment.

[Jump to the instructions](#instructions)

## Previous Experiences Setting Up Jekyll

Before I go into the process for moving my Jekyll development environment to Docker, I'd like to go into a little detail about my previous experiences with Jekyll. As simple as Jekyll is, I've never had a totally smooth experience getting it up and running on a new computer.

My issues have perhaps been compounded by the fact that I've at various times been working with Jekyll on macOS, Ubuntu Linux, and Windows 10 (including attempting to use the Windows Subsystem for Linux). I've always ended up getting it to work, but there has usually been at least one snag along the way, whether it's getting the correct version of Ruby installed or working around issues with the RubyGems SSL certificate.

## Why Move to Docker Now?

There are two main reasons why I decided now was the right time to start using Docker for my Jekyll development environment:

1. I'm currently working on a proposal for my day job that features containerization with Docker as a key component of our solution.
2. I just bought a new laptop and am trying to be more thoughtful about how I set up my local environments.

## Instructions

Follow these steps to move your existing Jekyll site to a Docker-based install.

1. Install Docker, if you haven't already.
2. Clone your existing Jeykll site repo, if you do not already have a local copy of it.
3. Add a `docker-compose.yml` file to the root folder of your repo with the following contents:

   ```
   jekyll:
       image: jekyll/jekyll:pages
       command: jekyll serve --incremental
       ports:
           - 4000:4000
       volumes:
           - .:/srv/jekyll
   ```

4. Open a terminal and navigate to your site's directory.
5. Run `docker-compose up`.
6. Open your site at `http://localhost:4000`.

And... that's it. I had my site up and running in almost no time, and with no issues.
