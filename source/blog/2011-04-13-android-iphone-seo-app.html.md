---
title: 'Android+iPhone SEO App'
date: 2011-04-13
tags: android, android-mobile, android-seo-app, iphone, iphone-mobile, iphone-seo-app, mobile, mobile-app, sem, seo, titanium, titanium-2
---

I just released a new iPhone SEO app and Android SEO app called [SEMTab SEO
Pro](http://semtab.com). The basic idea is to keep a list of domains saved, and
check SEO/SEM stats like Google PageRank (PR), backlinks, Alexa rank, etc. and
Social share information from Twitter, Facebook, and Delicious.  more


[![iPhone SEO App - Domain List](http://www.vancelucas.com/wp-content/uploads/2011/04/ss1-list-200x300.png)](http://www.vancelucas.com/wp-content/uploads/2011/04/ss1-list.png)
[![iPhone SEO App - Web SEO Stats](http://www.vancelucas.com/wp-content/uploads/2011/04/ss2-web.png)](http://www.vancelucas.com/wp-content/uploads/2011/04/ss2-web.png)
[![](http://www.vancelucas.com/wp-content/uploads/2011/04/ss3-social1-180x300.png)](http://www.vancelucas.com/wp-content/uploads/2011/04/ss3-social1.png)

The first two pictures are of the iPhone app, and the last one is of the
Android app. SEMTab SEO Pro was built with  [Titanium](http://appcelerator.com)
using all native cross-platform UI controls, so it builds both the iPhone and
Android app from a single codebase and a single development effort.

The nice thing about SEMTab is that it makes extensive use of Titanium's event
system to fire off simultaneous asynchronous HTTP requests to the various web
services and APIs to fetch data about the current domain you are checking. This
prevents the application from locking up while fetching rank or share
information, and it prevents the HTTP requests from stacking up in a queue and
waiting for the ones in front of it to finish. The end result is a pretty slick
& simple SEO app that gets the results you want quickly, without feeling
sluggish or unresponsive.

Check out
[SEMTab SEO Pro](http://semtab.com) in the
[App Store](http://itunes.apple.com/us/app/semtab-seo-pro/id427828181?mt=8&ls=1) or the
[Android Market](https://market.android.com/details?id=com.actridge.semtab.seopro)
when you get the chance — and don't forget to leave some feedback with a rating.
