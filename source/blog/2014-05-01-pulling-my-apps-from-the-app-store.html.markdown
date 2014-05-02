---
title: 'Pulling My Apps From The App Store: Lessons Learned'
date: 2014-05-01 02:31 UTC
tags: ['apps', 'mobile']
---

![Pulling My Apps From The App Store](/images/posts/2014/appstore/itunesconnect-unpublished-apps.png)

Last night, I decided to pull my two iOS and Android apps [SEMTab SEO
Pro](/blog/android-iphone-seo-app/) and my very first app, [AutoRidge
Lite](/blog/zero-to-app-in-two-weeks-with-titanium/) from the App Store and
Google Play. It was a difficult decision to make, because they were my first
two apps, and I now have _no published mobile apps_ to show potential freelance
clients when they ask for app examples. This does limit my options, but now is
a great time to make this move since I just started a full-time long-term
contract and won't be actively looking for extra work right now.

## Showcasing Quality

With the [recent shutdown of my company](/blog/funemployed/), I feel like I'm
writing a new chapter in my life. Having any app in the app store just to say I
have one is no longer enough. The bar is higher now. **I want a high-quality,
well-rated app that people genuinely like and want to use**. I want something I
can really be proud of. It doesn't have to be complicated or difficult, or even
do anything fancy. It just has to look good, and do one thing well -- and _not
crash randomly_. Difficult task, I know.

## What A Bad App Looks Like

To provide an example of the kind of results a bad quality app will give you,
look no further than my own app - my only paid app - one I just pulled from the
stores - [SEMTab SEO Pro](http://semtab.com/). This app was ill-conceived and
hastily crafted. The UI wasn't thought out very well, and neither was the
stability of the source data. The code I used to fetch Google's PR ranking was
frequently wrong (I apparently used the wrong black magic incantation to summon
the accurate result), and link counts from Facebook and Twitter were
different than what were reported by other (more accurate) tools and by the
platforms themselves (turns out the API I used was way outdated).

### Ratings

On Google Play, SEMTab had an abysmal rating of 1.6. This is primarily due to
the app crashing or just plain not working on a few Android devices that I
was not able to test on, or were not actually supported (like the "Rubbish"
review you see for it not working on the Galaxy Tab - one of the first Android
tablets that liked to pretend it could run normal Android apps just fine).

![Google Play Rating - An abysmal 1.6](/images/posts/2014/appstore/semtab-googleplay-ratings.png)

On the App Store, the app was a lot more stable, but still got a bad rating for
its terrible accuracy.

![App Store Rating - An not-much-beter 2 stars](/images/posts/2014/appstore/semtab-appstore-ratings.png)

### Revenue

The app was $1.99, and was on sale for $0.99 for an extended period
of time to try and boost sales. Obviously, this strategy doesn't work if your
app sucks and has a bunch of bad ratings.

Google Play Revenue

![Google Play Revenue 2011-2014](/images/posts/2014/appstore/semtab-googleplay-revenue.png)

App Store Revenue

![iTunes Connect Stats 2011-2014](/images/posts/2014/appstore/itunesconnect-proceeds-2009-2014.png)

A whopping **$127.19** from Google and **$83.01** from Apple, for a grand total
of **$210.20**. _In 4 years_. Ouch. Totally not worth the (admittedly small)
effort I put into it.

### Why I Pulled SEMTab SEO Pro

The reasons here are prety obvious, right? At this point, the ratings and
performance of this app are so bad that I don't even _want_ it in my portfolio.
I didn't even want people to know I made this app, and was a bit embarrased to
mention it whenever the "what apps have you made?" question came up, fearful
people would see the terrible rating read all the horrible reviews. Good
riddance. _(And if you're wondering why I am publicly sharing all this now, it's
for accountability - I never want to make an app this bad again.)_

## What a Mediocre App Looks Like

I am definitely _more proud_ (or, at least, not embarrased) by my first app -
[Autoridge](http://autoridge.com/mobile). I was a _lot_ more torn
about taking down Autoridge than I was about SEMTab. The reviews of Autoridge
basically boil down to this:

 * The people who used the app and found relevant data for their vehicle's
   year/make/model **loved it**. It provided huge value for them.
 * The people who could not find their specific vehicle's year/make/model, or
   didn't find any (or very few) relevant parts for their vehicle mostly
   trashed it as incomplete, inaccurate or unreliable.

### Poor Early Titanium Android Support

And as in the case of SEMTab, there were also quite a few Android users that
the app didn't work for at all (freezing or crashing). I mostly attribute this
to the very poor early [Appcelerator Titanium](http://www.appcelerator.com/)
Android support. Both these apps were developed and released at least 3 years
ago on Titanium versions 1.5.x - 1.7.x, and a lot has changed for the better
since those dark ages with Titanium (we have have an MVC framework called
Alloy, and Titanium is at 3.2.x). From my experience with Alloy, I am positive
that updated versions of these apps would elimate all the problems on Android.

That said, it's pretty disharenting when you develop an Android app with an
intermediate platform like Titanium, test it locally with your own Android
device and emulator working perfectly, and then release it to a tsunami of
1-star reviews and reports of random freezes and crashes on various Android
devices that you can't do anything to fix.

### Ratings

On Google Play, Autoridge Lite had a **very average rating of 3.1**. I am
actually suprised it was this high with the number of ratings reporting freezes
and crashes, but like I said, the number of people who it did work for
generally loved it. You can see this wide rating distribution reflected in the
data. This saddens me a bit, because the rating could have easily been 4+ in
Google Play if it worked consistently across all Android devices.

![Google Play Rating - A very average 3.08](/images/posts/2014/appstore/autoridge-googleplay-ratings.png)

The App Store rating was pretty much the same story, with all the negative
reviews questioning data completeness and accuracy:

![App Store Rating - An not-much-beter 2 stars](/images/posts/2014/appstore/autoridge-appstore-ratings.png)

### Distribution

The uptake of Autoridge Lite surprised me from the start. It definitely seems
like something people were ready to embrace if the data was more accurate,
complete, and up-to-date. I think overall, people really like the idea of using
an app instead of flipping through a greasy old parts book at the auto part
store.

App Store Distribution Units

![App Store Units - 2009-2014](/images/posts/2014/appstore/itunesconnect-units-2009-2014.png)

Google Play Installs

![Google Play Distribution - 2010-2014](/images/posts/2014/appstore/autoridge-googleplay-units.png)
![Google Play Active Installs - 2010-2014](/images/posts/2014/appstore/autoridge-googleplay-activeinstalls.png)

According to Google, there are **still 1,584 devices with Autoridge Lite
installed**. Definitely far better than SEMTab SEO Pro that had only
12 active installs. A lot of people still like this app.

### Why I Pulled Autoridge Lite

The decision to pull Autoridge from the App Store and Google Play was much
harder than with SEMTab, but ultimately, it came down to an issue of time,
energy, and focus:

 * Autoridge Lite has a [webservice backend](http://autoridge.com) that I also
   built and maintain, and this costs time and money.
 * The vehicle data has a low degree of accuracy with no automatic error
   correction, and would take a significant re-investment of time and energy to
   correct. I would have to research new sources of auto part information and
   build web scrapers/parsers for each of them, as well as implement better
   processes for checking for updates and invalidating bad data.
 * I want to completely re-code the app, and have wanted to for a while. So
   doing this plus all the webservice updates too would probably take longer than
   the original 2 weeks the entire project took initially. And since I can't
   update the app only without fixing the data accuracy issue, I have to
   consider the whole picture.

In short - it would take too long to re-build the right way, and I don't
currently see it as worth the effort considering I haven't made any money from
it (and don't see any path to make significant money from it ever as a one-man
show).

## Conclusion

Although this choice was tough, and arguably should have happened a long time
ago, I am happy to be doing it now. **The first step towards finding something
that works is identifying and eliminating the stuff that doesn't**. The worst
thing that can happen is to split your time, energy, and focus across so many
projects that you do them all badly. This is exactly what I feel I have done.
**I want to be able to focus on my next project, I want it to be
high-quality, and I want it to have a clear revenue path**. This whole "making
money with software" path is not an easy one, so I want to give myself the best
chance possible by concentrating on the next big thing, and making a better
product than I've ever made before. That's what this move is all about.


