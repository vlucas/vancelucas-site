---
title: 'Nginx + PHP-FPM Blank Pages with PHAR Packages'
date: 2012-03-07
tags: nginx, phar, php, php, php-5-3, php-fpm, servers, suhosin
---

Ran into this issue when setting up a new VPS for
[AutoRidge](http://autoridge.com). This happens when using Nginx and PHP-FPM
with PHP 5.3+ and the Suhosin patch when trying to run a PHP script using a
PHAR package. From what I can gather, the Suhosin patch basically blocks PHP
include/require functions from executing files ending with .phar, which results
in a PHP segfault that leaves no trace of any error at all. This is what makes
this error so frustratingly difficult to track down - there is no trace left in
any logs about what is happening or that any PHP error even occurred at all.

The solution is to open your "suhosin.ini" file to ensure the Suhosin patch is
allowing PHP to open and execute PHAR files.

Mine was at:

```
/etc/php5/conf.d/suhosin.ini
```

If your suhosin config file is not there and you don't know where it is, run this:

```shell
locate suhosin.ini
```

Find the config key `suhosin.executor.include.whitelist` and add "phar" to it.

```ini
suhosin.executor.include.whitelist ="phar"
```

Then restart PHP-FPM and Nginx and you should be good to go!

```shell
/etc/init.d/php5-fpm restart
/etc/init.d/nginx restart
```
