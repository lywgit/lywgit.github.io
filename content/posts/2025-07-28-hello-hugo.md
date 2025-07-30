+++
date = '2025-07-28T12:32:06+08:00'
title = 'Hello Hugo'
tags = ['tech', 'hugo']
+++

I started learning Backend knowledge on [Boot.dev](https://www.boot.dev) a while ago, and somewhere in a lesson it mentioned that their blog was built using [Hugo](https://gohugo.io). Later when I wanted to update my personal site, I did some research and eventually decided to give Hugo a go! 

My site was originally generated using [Jekyll](https://jekyllrb.com), and I have no problem with it except I know nothing about Ruby on which Jekyll was built. I am well awared that it is the content of a site that matters, not the tool that builds it, but it just feel better to (at least potentially) be able to understand what's going on under the hood. Hugo was based on Go, and I like Go.

Like Jekyll, Hugo comes with a lot of themes from the community and they can be found under this [theme site](https://themes.gohugo.io/). It took me some time to goof around before settling down on the [Congo](https://github.com/jpanther/congo) theme. Here's what works for me:

- The simple but enough appearance. Check out Congo's [demo](https://jpanther.github.io/congo/) site.
- The [repository](https://github.com/jpanther/congo) is highly rated (1.5k at the time of this post), and is actively maintained.
- Support dark and light mode (the toggling button is at the bottom right of the page)
- Support mathmatical notation rendering: hello \\(\LaTeX\\)
- Responsive layout design, meaning the site will also look nice on a cell phone.
- 中文顯示沒問題（跟 theme 應該沒關係，但還是要試試）

To get a Hugo site up and running locally is simple. To customize stuffs and serve it on a Github page is harder, but not a problem if you are a developer. The experience might not be smooth if you are new to programming and Guthub (I remember the same when I first used Jekyll), but the docs are there to help. So far so good.


{{< katex >}}