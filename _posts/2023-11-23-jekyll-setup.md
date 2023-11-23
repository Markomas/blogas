---
title: Jekyll setup
layout: post
---

So, as first post, i will describe how I set up this page (it will be short, because I already forgot some things 😛

# Jekyll install journey:
I followed these instructions:
https://jekyllrb.com/docs/

But, for some reason, I used some old instructions and spent understanding strange ruby/gem errors. Never used it, so it was mistery for me.
But some how, I got it working.

Then, because I am lazy and sometimes I prefer a nice GUI over editing config files, I Google searched for jekyll admin. 
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

I was greeted with similar page:
![Administrator](/assets/images/posts/2023-11-23-jekyll-setup/admin.png)

> Well after using it, I will probably will try to find something better, because of image management  to blog post isn't smooth.
> But for now it will be fine.

# Time to publish it for free (well I have domain, but hosting is free)
So how we get free hosting, that we can trust (well not fully, but it should be stable enough for me)

It's **github**, you will ask how? Github is just for soure code, you still need hosting. But nah, github has neat feature called 'Pages'.

I will create, github public project here: https://github.com/Markomas/blogas
Then I will go Settings -> Pages
![Administrator](/assets/images/posts/2023-11-23-jekyll-setup/pages.png)

And my domain registrar:
![iv.lt](/assets/images/posts/2023-11-23-jekyll-setup/iv.png)
