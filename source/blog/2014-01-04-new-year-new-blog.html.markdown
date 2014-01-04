---
title: New Year, New Blog
date: 2014-01-04 04:57 UTC
tags:
---

It's a whole new year, and I've got a whole new blog. This time around, I knew
I wanted a static blog generator instead of a WordPress site (they are a little
more developer friendly, and there are no security vulnerabilities with static
HTML), and I've been window shopping a bit. A self-hosted blog was important to
me since I want to make sure I will always own and control all my own content.

### The Options

Though popular, [Octopress](https://github.com/imathis/octopress) was out,
because of my experience using it on [OKC.js](http://okcjs.com). It is
difficult to customize, and the Octopress code is mixed in with your blog and
website content, making upgrading difficult as well.

Using [Jekyll](http://jekyllrb.com/) was very tempting - and I almost used it,
but I decided to take a look at another option first - and I'm very glad I did.

### Enter Middleman

I ended up going with [Middleman](http://middlemanapp.com/) for this blog. We
just re-launched the [Brightbit](http://brightbit.com) website with it, and
Joshua (my design co-founder) was raving about it, so I had to give it a shot.
Both Jekyll and Octopress use [Liquid](http://liquidmarkup.org/) templates,
which are a learning curve if you've never used them. I personally don't like
the syntax, so I wasn't too keen on doing a lot of layout customization with
it.

Middleman, however, is different. Middleman offers so much more flexibility -
pure Ruby code, Sass, the option to use Slim or Haml for templates, blog posts,
and pages, and the same asset pipeline that Rails has for combining and
compressing your CSS and JavaScript into a single file for production
deployment. Layout customization is also easier and better feeling in general.
The last, and perhaps biggest reason Middleman is a winner is that Middleman
exists entirely inside it's own self-contained gem. Your site's project folder
has only what it should - your site's content. There is no Middleman cruft in
there that you have to bring along, and upgrades are clean and brainless since
Middleman is a gem.

### Wrapping Up

Importing all my old posts from WordPress and converting them all to markdown
was a bit tricky and time-consuming (though
[wp2middleman](https://github.com/mdb/wp2middleman) did most of the heavy
lifting), but I'm glad I did it. Here's to a great 2014 on a great new (and
much better looking) blog.

