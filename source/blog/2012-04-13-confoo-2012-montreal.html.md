---
title: 'Confoo 2012 Montreal'
date: 2012-04-13
tags: speaking-engagements
---

My second year speaking at
[Confoo](http://confoo.ca) was more fun than my first. This year I met a lot of new people and had a lot of interesting discussions, particularly in the CMS room and at lunch after my
[Stackbox CMS](http://stackboxcms.com) presentation. The whole philosophy and approach of Stackbox seemed to have stuck a chord with other people passionate about CMSes and a lot of discussion emerged about different CMS concepts and how to integrate them into the various CMSes around. The whole edit-on-page approach of Stackbox isn't new, but it's striking how much simpler it is for the end user than most existing CMSes that are used today -
[Drupal](http://drupal.org),
[Joomla](http://joomla.org), and
[Wordpress](http://wordpress.org) included. I think 
**all**
 CMSes should strive to enable on-page and in-place editing wherever possible - it really makes a huge difference in usability.

The second talk I gave at Confoo was 
[Hierarchical MVC (HMVC) - What, Why, and How](http://confoo.ca/en/2012/session/hierarchical-mvc-hmvc-what-why-and-how) - an architectural talk I have been wanting to give at several conferences for a little while now (but had until now not been accepted). The talk was very well received, and hopefully helped at least a few people though some of the tougher architectural decisions they might be facing in their own projects. The gist of the talk is that HMVC can help break up code into "widget" type blocks, and can go further down the path of fully separating concerns than more strict traditional MVC can. HMVC is all about code re-use across multiple places, like a comment module that is dispatched to anywhere you display comments across multiple types of content (blog posts, articles, pages, events, etc.). Traditional MVC forces you to use view partials, different layouts, duplicate code, or some other separate widget system to achieve the same level of flexibility you get from HMVC.

One of the best things I like about Confoo is the sheer diversity of the schedule. There were 10 tracks this year, with talks spanning across all types of technologies, markets, and languages, like PHP, Ruby, Python, .NET, Java, and JavaScript to Scaling, Startups, CMSes and Agile. Confoo is the most technologically diverse conference I have had the privilege of speaking at, and I benefit from that diversity every year by expanding my horizons a little bit. While I was there, I attended a
[talk](http://confoo.ca/en/2012/session/renee) on a Ruby framework called
[Renee](http://reneerb.com/) by
[Joshua Hull](https://github.com/joshbuddy). Even though most of my background is in PHP and I was presenting with PHP examples and projects for my talks, I use Ruby on Rails quite a bit for client work at my company 
[Brightbit](http://brightb.it) and was working on a Rails API at the time, so I thought I'd check it out. I had been thinking about better REST frameworks beyond MVC for a little while, and the talk inspired me to try the same nested callback style with closures in PHP, and I started hacking together the very first (crude) implementation of
[Bullet](https://github.com/vlucas/bulletphp) on the plane ride back home the next day, just to see if it was possible. I had the basic concept working well in under an hour thanks to PHP 5.3's awesome closures. That's why I like and value the diversity at Confoo so much. You get to see things that are going on in other languages and expand your horizon a bit, then you can bring those benefits back to another language you work in, and benefit even more people. I've fleshed out Bullet a little more now and made it a proper project, but the details of that are for another blog post. For now, I leave you with the slide decks I presented at Confoo.

### Slide Decks
[Stackbox CMS Slides](https://speakerdeck.com/u/vlucas/p/stackbox-cms-next-generation-content-management)


[Hierarchical MVC (HMVC) Slides](https://speakerdeck.com/u/vlucas/p/hierarchical-mvc-what-why-how)
