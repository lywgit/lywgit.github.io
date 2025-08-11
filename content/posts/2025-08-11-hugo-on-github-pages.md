+++
date = '2025-08-11T09:12:50+08:00'
title = 'Hugo Site on Github Pages'
tags = ["tech", "hugo"]
+++

Building a website that only you can view is not very fun. Nowadays there are plenty of resources you could use to host your site for free. I use GitHub Pages because I only need a static site and GitHub feels reliable and clean.

Unlike Jekyll, Hugo does not work out of the box with GitHub Pages, which was a potential concern when I decided to make the switch. 
[Hugo's official documentation for hosting on GitHub Pages](https://gohugo.io/host-and-deploy/host-on-github-pages/) doesn't look very friendly at first due to the lengthy workflow file you need to define the build and deploy process. Fortunately, following the steps and copy-pasting works just fine. I didn't dig into the details of the workflow too much, which might be quite educational. 

Note that in [Congo's documentation](https://jpanther.github.io/congo/docs/hosting-deployment/), the workflow file looks simpler mainly because of the `peaceiris/actions-hugo` action that probably wraps several things up for you. But I stuck to Hugo's original version which felt a bit more native.