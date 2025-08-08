+++
date = '2025-08-08T00:00:00+08:00'
title = 'Moving Posts to Hugo with Commit History'
tags = ['tech', 'hugo', 'git']
+++

## Front Matter Matters

I have some earlier posts on my Jekyll site and I naturally wanted to move them to my new Hugo site.
This was really a trivial task but I got tripped because I was careless about the different [front matter](https://gohugo.io/content-management/front-matter/) formats. 

This is a `toml` format example:
```
+++
date = 2024-02-02T04:14:54-08:00
title = 'Example'
+++
... post content ...
```

This is a `yaml` format example (like Jekyll):
```
---
date: 2024-02-02T04:14:54-08:00
title: Example
---
... post content ...
```

And this is my initial failing example (I believe you can spot the reason): 
```
# THIS IS WRONG!
---
date = 2024-02-02T04:14:54-08:00
title = 'Example'
---
... post content ...
```

Note there are actually commands to convert matters of your existing posts [from yaml to toml](https://gohugo.io/commands/hugo_convert_totoml/) or [from toml to yaml](https://gohugo.io/commands/hugo_convert_toyaml/). I never tried them because I found them after I have done the update manually.

Getting the front matter right is almost enough to serve my posts on the Hugo site correctly. The only extra work in my case is to [enable math rendering](https://jpanther.github.io/congo/samples/mathematical-notation/) since some of my posts need it.


## Keeping the Commit History 

With the new Hugo site running and all posts migrated and served, I could now simply wipe out the old repository and replace it with the new one, but I actually wanted to keep the old commits as part of the history.
Since I have two separate repositories with unrelated commits, this is a bit tricky. 

Consulting with Gemini led me to the "merge with `--allow-unrelated-histories`" approach and this is the final result I arrived at:

![commit history](commit-history.png "Screenshot of commit graph after merging (the unrelated) local Hugo commits into the main site repository")


And here are the steps:

1. Clone the site repository and cd into the cloned directory
    ```bash
    git clone <site-repo>  # Ex: https://github.com/lywgit/lywgit.github.io.git 
    cd <site-dir>          # Ex: lywgit.github.io
    ```
2. Create and switch to a new branch `hugo-migration` to work on (safer but not necessary)
    ```bash 
    git checkout -b hugo-migration
    ```
3. Remove all old content (not the hidden `.git`). Commit the change 
    ```bash
    rm -rf *
    rm .gitignore # explicitly remove .gitignore
    git add .
    git commit -m "remove entire Jekyll site"
    ```
4. Add the new locally developed Hugo site as a remote source 
    ```bash
    # Yes, you can use a local dir as remote!
    git remote add hugo-source <path-to-hugo-site-local-repo> # Ex: my-hugo-site-dir/
    git fetch hugo-source
    ```
5. Merge the content with `--allow-unrelated-histories` flag
    ```bash
    # The merge should succeed without conflict
    git merge hugo-source/main --allow-unrelated-histories
    git commit -m "Merge Hugo site content with existing repo history"

    ```
6. Push the `hugo-migration` brach back, create a pull request and merge back to the main branch
    ```bash
    git push origin hugo-migration
    ```
7. Use the github website to create a pull request and merge back to the main branch
8. Switch back to main branch and pull the latest
    ```bash
    git checkout main
    git pull origin main
    # optionally remove the working branch and hugo-source remote branch
    git branch -d hugo-migration
    git remote remove hugo-source
    ```

Note: Since I am working alone, the pull request stuff is probably not necessary. I could simply merge the `hugo-migration` branch into the `main` branch locally and then push it back, but it was fun anyway.