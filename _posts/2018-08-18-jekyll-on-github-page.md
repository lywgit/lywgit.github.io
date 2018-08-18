---
title: Hosting Jekyll Site on Github-Page 
categories: jekyll
tags: jekyll
---

## Reference

* [Jekyll home](https://jekyllrb.com/docs/home/)
* [Using Jekyll as a static site generator with GitHub Pages](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/)
* [mmistakes/so-simple-theme](https://github.com/mmistakes/so-simple-theme)

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
# cd into working directory
$ jekyll new .
```

### Setup for the so-simple-theme

Check out [so-simple-theme](https://github.com/mmistakes/so-simple-theme) and follow the part of howting on github-page.

1. Edit Gemfile (follow the intruction in the file): 
    * Comment out: `gem "jekyll", "~> 3.8.3"`
    * Uncoment: `gem "github-pages", group: :jekyll_plugins`
2. Edit _config.yml: 
    * Comment out: `theme: minima` 
    * Add `remote_theme: "mmistakes/so-simple-theme"`

At this time, pushing the content to github should successfully generate a page.


## Serving locally with Jekyll

Serve and view the site locally

```console
bundle exec jekyll serve
```
And then go to [http://localhost:4000/](http://localhost:4000/) with a browser.

This will probably fail when running for the fisrt time. Update gem and thing should work out.

```console 
sudo bundle update
```

