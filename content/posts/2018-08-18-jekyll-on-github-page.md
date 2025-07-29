+++
date = '2018-08-18T00:00:00+08:00'
title = 'Jekyll on Github Page'
tags = ['tech', 'jekyll']
+++

On Auguts 6 2018, I come back to the idea of using Jekyll to setup a site to host some of my notes. Both jekyll and the so-simple-theme have evolved since I first used them on March 2017. I therefore decide to build the site again with the updated theme.


## Github-Page 

Follow instruction on [the site](https://pages.github.com/) to create a repository for hosting. I use a user page (not a project page).

## Install Jekyll

Follow instruction on [Jekeyll installation page](https://jekyllrb.com/docs/installation/). Will need `sudo`.

```console
$ sudo gem install bundler jekyll
```


## Start a Jekyll site for github-page

Start a fresh site
 
```console
# cd into the working directory
$ jekyll new .
```
Note the final `.` (current directory).

### Setup for the so-simple-theme

Check out [so-simple-theme](https://github.com/mmistakes/so-simple-theme) and follow the part of howting on github-page.

1. Edit Gemfile (follow the intruction in the file): 
    * Comment out: `gem "jekyll", "~> 3.8.3"`
    * Uncoment: `gem "github-pages", group: :jekyll_plugins`
2. Edit _config.yml: 
    * Comment out: `theme: minima` 
    * Add `remote_theme: "mmistakes/so-simple-theme"`

At this time, the content is enough for github to generate a page. 


## Serving locally with Jekyll

It is probably necessary to update gem when running for the fisrt time
 
```console 
$ sudo bundle update
```

Afterward, should be able to serve and view the site locally

```console
$ bundle exec jekyll serve
```
And then go to [http://localhost:4000/](http://localhost:4000/) with a browser.

## 




## Reference

* [Jekyll home](https://jekyllrb.com/docs/home/)
* [Using Jekyll as a static site generator with GitHub Pages](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/)
* [mmistakes/so-simple-theme](https://github.com/mmistakes/so-simple-theme)
