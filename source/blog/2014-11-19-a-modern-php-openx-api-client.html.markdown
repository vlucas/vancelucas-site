---
title: A Modern PHP OpenX API Client
date: 2014-11-19 21:58 UTC
tags: ['php', 'openx', 'api']
---

I released a new [OpenX REST API
Client](https://github.com/vlucas/openx-oauth-client) that works with the
newest OpenX v4 REST API. It uses [Guzzle v4.x](https://github.com/guzzle/guzzle/tree/4.2.3) and the
[oauth-subscriber plugin](https://github.com/guzzle/oauth-subscriber). It is
[available on Packagist](https://packagist.org/packages/vlucas/openx-oauth-client),
uses the PSR-4 autoloader, and is properly namespaced. It took a bit of effort
to put together, so I hope you enjoy using it, and I hope it saves you a lot of
time.

READMORE

## What About the Official Client?

I didn't really want to have to write a custom client library to use the OpenX
API, but the [official PHP
library](https://github.com/openx/OX3-PHP-API-Client) was very outdated, both
in terms of code and composer autoload-friendliness. It has been updated to
work with v4 API, but the README and docs in the library itself don't say that,
and only mention API v3. It has no namespaces, and can't be autoloaded
just by using the class names.

Beyond just code and style issues, the offical client is based on Zend
Framework 1 components, when ZF2 has been around for a few years now. I also
have a bias against ZF components, because they are not very de-coupled, and
have a tendency to [require nearly the rest of the entire
framework](http://paul-m-jones.com/archives/4176) when using them. I don't want
to include half of Zend Framework just to make a few API calls. Now, I don't
have to!


