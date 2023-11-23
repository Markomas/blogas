---
title: Jekyll setup
date: 2023-11-23 00:00:00 Z
layout: post
---

So, as the first post, I will describe how I set up this page (it will be short because I already forgot some things 😛

# Jekyll install journey:
I followed these instructions:
https://jekyllrb.com/docs/

But, for some reason, I used some old instructions and spent understanding strange ruby/gem errors. Never used it, so it was a mystery to me.
But somehow, I got it working.

Then, because I am lazy and sometimes I prefer a nice GUI over editing config files, I Google searched for Jekyll admin. 
And what I found: https://github.com/jekyll/jekyll-admin

Just added these to Gemfile
```
gem 'jekyll-admin', group: :jekyll_plugins
```

And ran these commands:

```
bundle install
bundle exec jekyll serve --livereload
```

And after opening:
http://localhost:4000/admin/

I was greeted with the similar page:
![Administrator](/assets/images/posts/2023-11-23-jekyll-setup/admin.png)

> Well after using it, I will probably try to find something better, because image management for blog posts isn't smooth.
> But for now it will be fine.

# Time to publish it for free (well I have a domain, but hosting is free)
So how do we get free hosting, that we can trust (well not fully, but it should be stable enough for me)

It's **github**, you will ask how? Github is just for source code, you still need hosting. But nah, github has a neat feature called 'Pages'.

I will create, github public project here: https://github.com/Markomas/blogas
Then I will go to Settings -> Pages
![Administrator](/assets/images/posts/2023-11-23-jekyll-setup/pages.png)

And my domain registrar:
![iv.lt](/assets/images/posts/2023-11-23-jekyll-setup/iv.png)
