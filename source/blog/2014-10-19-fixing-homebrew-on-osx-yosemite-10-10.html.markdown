---
title: "Fixing Homebrew on OSX 10.10 Yosemite"
date: 2014-10-19 23:25 UTC
tags: ['programming', 'apple', 'osx']
---

If you upgraded to OSX 10.10 Yosemite, and now have a broken
[homebrew](http://brew.sh/), fear not
- the homebrew team has already fixed this!

Luckily, the steps to fix it are fairly simple.

First, update homebrew via `git`:

```
cd /usr/local/Library
git pull origin master
```

Next, use homebrew to update and clean your installed packages:

```
brew update
brew prune
brew doctor
```

Now you should be all set!

Footnote:

I originally found (and [tweeted
 about](https://twitter.com/vlucas/status/523975518421397504)) [this
article](http://jcvangent.com/fixing-homebrew-os-x-10-10-yosemite/) when
searching for a fix, but ran into more issues after editing the `brew.rb` file,
and eventually came to the solution of updating homebrew itself after seeing
that the homebrew team had fixed the issue themselves.

