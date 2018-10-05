---
title: Customizing Navigation Bar
layout: post
categories: blog
tags: jekyll
---

## Adding a new category

As explained in the [theme-setup](https://mmistakes.github.io/so-simple-theme/theme-setup/) page of so-simple-theme, one can edit `_data/navigation.yml` to add links to your top navigation bar. 

I would like to have a new top link named "Notes" to gather posts in my category "notes", so I edit `_data/navigation.yml` to add the following: 

```yml
- title: Notes
  url: /notes/
```

This information is used in `_include/navigation.html` to create a link on the top navigation bar called "Notes". 

A few details may help understand what is going on.

### The navigation link

The title and url above are accessed through variable `site.data.navigation` in `_include/navigation.html`. For a relative url like this (no "http" in the front, just /notes/), it will be prepended with the (site) url in file `_config.yml` to form a complete address. The latter is accessed through variable `site.url`. In my case, this will be https://lywgit.github.io + /notes/ = https://lywgit.github.io/notes/.

In short, the url setting decides that the "Notes" link in the navigation bar will visit https://lywgit.github.io/notes/ in the final generated site, so there better be something there. This is why you need to create a new directory named notes/ under the main project, and put an index.md file in it. You can copy blog/index.md to notes/index.md and edit it to change the category name from "blog" to "notes". This is also described in the [theme-setup](https://mmistakes.github.io/so-simple-theme/theme-setup/) page, look for "Categories" in that page.

Note the line `{%raw%}{% for post in site.categories.notes %}{%endraw%}` will now loop over those posts with category name "notes". and you should therefore use "notes" as the category name in the front matter of a post for it to show up here. 


### The article address

One may notice that there are subdirectories under _posts/, such as articles/ and blog/. So do these directory names decide the link of each posts? It turns out not. The directory name here does not affect the article address in the generated site. The address is decided by the `permalink` variable in `_congif.yml`. By default this is `permalink: /:categories/:title/`, so the post address is generated according to the category and the title (and the site url, to be precise). Therefore as long as the front matter says `categories: notes`, it will be under "https://lywgit.github.io/notes/", regardless of the substructure under your `_post/` directory. 

### In short 

If you use `categories: abc` for a post, the corresponding page will be generated and found under "https://lywgit.github.io/abc/article\_name". Whether you have ever created a directory called abc/ under your main project or under your _post/ directory does not matter. You just need to make sure that the navigation link setup (the url in `_data/navigation.yml`) is consistent with the category name in the front matter of a post. 


---

<h3 style="color:brown"> Update (2018-10-04) </h3> 

See [post-url-generated-by-jekyll]({% post_url 2018-10-04-post-url-generated-by-jekyll %}) for an update on the url stuff.  


