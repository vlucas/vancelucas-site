---
title: Using Dokku to Deploy PHP Applications on DigitalOcean
date: 2014-02-08 19:48 UTC
tags: ['server', 'vps', 'php']
---

Want a Platform-as-a-Service setup like [Heroku](http://heroku.com) on your own
[$5/month VPS from DigitalOcean](https://www.digitalocean.com/?refcode=b814bf3a4035)? Look no
further than
[Dokku](http://progrium.com/blog/2013/06/19/dokku-the-smallest-paas-implementation-youve-ever-seen/)
- a set of scripts built on [Docker](https://www.docker.io/) and Heroku's own
buildpacks. After this setup, you're just one `git push` away from deploying
your app to your own server.

## Step 1: Create a new Droplet with Dokku

DigitalOcean has a great guide on [how to use the DigitalOcean Dokku
Application](https://www.digitalocean.com/community/articles/how-to-use-the-digitalocean-dokku-application?refcode=b814bf3a4035),
so there is no sense in repeating the steps here. Follow the steps in that
article, and then come back here. **There are issues I ran into after the Dokku
setup that are important steps not to skip**. So be sure to come back here
before trying to deploy your PHP application.

## Step 2: Setup Swap Space

DigitalOcean boxes don't come with any disk swap space configured by default,
but Dokku uses some when deploying your apps, so you need to [configure some
using this
guide](https://www.digitalocean.com/community/articles/how-to-add-swap-on-ubuntu-12-04?refcode=b814bf3a4035)
before your first `git push` or you may run into errors (I sure did). The guide
says Ubuntu 12.04, but it works on 13.04 too, so no worries. Do this and then come back. I'll wait.

## Step 3: Deploy Your App

Although the setup is not fully complete, try and deploy your app now to make
sure your dokku setup is working properly. You also need to deploy your app now
so you will have a container ready for the next steps.

```
git remote add dokku dokku@yourdomain.com:app_name
git push dokku master
```

The ouput of the `git push` will let you know if your deploy was successful or
not. If you have a successful deploy, but get a "502 Bad Gateway" response from
Nginx, continue with the steps below.

## Step 4: Install the user-env-compile Plugin

Install the [user-env-compile
plugin](https://github.com/musicglue/dokku-user-env-compile). This will allow
you to set environment variables on your app that are available to be used at
build time. **This is important for the next step**.

## Step 5: Use CHH's PHP Buildpack

Now that you have the `user-env-compile` plugin installed and have created your
app's initial container, you can set environment variable configuration values
on it. To specify what buildpack you want to use, set the `BUILDPACK_URL` ENV value.

```shell
ssh dokku@<yourserver.com> config:set <app_name> BUILDPACK_URL=https://github.com/CHH/heroku-buildpack-php
```

You should see output like this:

```shell
-----> Setting config vars and restarting <app_name>
BUILDPACK_URL: https://github.com/CHH/heroku-buildpack-php
-----> Releasing <app_name> ...
-----> Release complete!
-----> Deploying <app_name> ...
-----> Deploy complete!
```

NOTE: ANY Heroku buildpack will work, so if you wnat to use another one, you
are free to do so, although it is very doubtful you will find a better one for
PHP :).

## Step 6: Set your Document Root

Now open your `composer.json` file, and add the `extra` configuration block to
specify your `document-root` and `index-document` so the PHP buildpack will
know where to serve your files from.

```json
{
    "require": {
        "php": ">=5.4.0",
        "vlucas/bulletphp": "~1.3.x"
    },
    "extra": {
        "heroku": {
            "framework": "slim",
            "document-root": "web",
            "index-document": "index.php"
        }
    }
}
```

Note: The `framework` key here is `slim`. This doesn't match [my framework
Bullet](http://bulletphp.com), but I have to put this in here because this maps
to the [nginx config file
used](https://github.com/CHH/heroku-buildpack-php/tree/master/conf/nginx). So
if I don't specify this here, the buildpack assumes a standard classic PHP
setup and will only execute `.php` files, and not rewrite all requests to the
main `index.php` file like most frameworks (including Bullet) require, causing
nginx to throw lots of "404 error" responses.

The configuration key says `heroku`, but this works for dokku too - remember
Dokku basically uses all the same basic things that Heroku does - buildpacks,
git based deploys, putting apps in their own sandboxed containers, etc.

## Step 7: Install Other Plugins Your App Needs

### Custom Domains

You will want the [dokku domains
plugin](https://github.com/wmluke/dokku-domains-plugin) so your app can use
domains instead of subdomains or ports.

Then add your domain to your your app using:

```
ssh dokku@<yourserver.com> domains:set <app_name> yourdomain.com api.yourdomain.com
```

### MySQL / MariaDB
If your app uses MySQL, install the [dokku MariaDB
plugin](https://github.com/Kloadut/dokku-md-plugin) (drop-in MySQL
Replacement).

Then create a database for your app using:

```
ssh dokku@<yourserver.com> mariadb:create <app_name>
```

This will create a `DATABASE_URL` environment variable that will be available
as a DSN string for your app to use to connect to the database via PDO or other
ORM that you may want to use. You can access this in PHP via `$_SERVER['DATABASE_URL']`.

### Other Plugins

If you use Redis, Memcached, or anything else, check the [Dokku Plugins
page](https://github.com/progrium/dokku/wiki/Plugins) and install whatever you
need.

## Step 8: Re-Deploy Your App

This time when you deploy your app via `git push`, it will use the custom buildpack you set in
Step 4. You should be able to view your app live on your custom port, subdomain, or
domain now, and all should be well.

Now sit back, relax, and enjoy your own mini-Heroku at a fraction of the cost!

