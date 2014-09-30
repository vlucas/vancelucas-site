---
title: An API is a Competitive Advantage
date: 2014-09-29 17:18 UTC
tags: ['business', 'api']
---

In this increasingly inter-connected world, APIs are becoming more and more
important as time goes on. This is especially true if you have a business that
requires integration of some sort, like metrics, notifications, integrated
access to other systems (like telephony), payments, etc.

Companies like [Stripe](https://stripe.com), [Amazon](http://www.amazon.com),
and [Twilio](http://www.twilio.com) have embraced the API-first approach, and
in many ways embody and epitomize this movement as a whole.

## Beyond Just Having An API

Just having an API is the obvious requirement for basic integrations. Going
further than that, however, is the thought that **your API can actually be a
key point of differentiation from your competitors**. Using this strategy
(creating a robust, easy-to-use API) can be especially effective when you are
going up against entrenched competitors, or when you are trying to make
something that is traditionally very hard, easy.

## Stripe And The Payments Industry

Ask any developer about online payment gateways, and they are likely to mention
[Stripe](https://stripe.com). Why? Because it was clear from the start that
they really cared about devleopers, and put high priority in their API. Not
only just creating an API - *because every online payment system has an API* -
but in creating a **very good API** that is robust, simple, well-documented,
and easy to use.

In contrast, many of Stripe's competitors are using SOAP APIs or an emulation of the
Authorize.net API. The API documentation typically exists only in PDF form, and
it's something that is mailed to you by the sales department. You're lucky if
you can find it on the website. Sales first and developers second is pretty
much the exact opposite approach that Stripe took by focusing on developers and
integrations first.

Here an [example from the Stripe
documentation](https://stripe.com/docs/api#create_charge) - it's just a simple
cURL call to charge a card, and returns a simple JSON response:

```
curl https://api.stripe.com/v1/charges \
   -u sk_test_BQokikJOvBiI2HlWgH4olfQ2: \
   -d amount=400 \
   -d currency=usd \
   -d card=tok_14i9vP2eZvKYlo2Cdr4h0oHs \
   -d "description=Charge for test@example.com"
 ```

Stripe did several things right here:

 - Provide a simple API with good documentation
 - Provide a fast on-boarding process with no red tape (rare for credit card processors)
 - Support subscription charges with no additional fees (also rare)
 - Target and market to developers
 - Good design and nice, clean merchant interface

Stripe's success is a combination of the above reasons as well as many other
factors, but **without a doubt their core product and main competitive
advantage is their API**. It shows in their overall developer experience, and
has played a large role in their success in stealing market share from
entrenched competitors like Authorize.net.

## Amazon and the Public Cloud

For many, [Amazon](http://www.amazon.com) is synonymous with cloud computing.
Many web hosts selling vitrualized servers came and went before Amazon got into
the game, but no one besides maybe [DigitalOcean](http://digitalocean.com) has
had a similar level of success doing so.

From the start of Amazon Web Services, **Amazon made it clear that they were a
platform for developers to build on top of**, and provided an API from day one.
So while many other virtual hosting providers existed, Amazon EC2 was one of
the only ones that developers could use to provision whole new servers with
automated scripts and zero manual intervention. The availability of APIs to
provision servers lead to the creation of businesses built on top of Amazon's
infrastructure, like [Heroku](https://www.heroku.com/) &ndash; who probably wouldn't
exist without Amazon's APIs.

[Rackspace](http://www.rackspace.com/), a much larger web host and significant
competitor, didn't launch a public API until years after Amazon did, but it
[was already too late, and they gave up significant market
share](http://www.cloudscaling.com/blog/cloud-computing/openstack-aws/) to
Amazon and Google Cloud Engine. **Amazon's API was its killer feature, and key
differentiator**. And we all [know how well that has gone for
them](http://venturebeat.com/2014/04/24/amazon-web-services-is-doing-so-well-it-has-nowhere-else-to-grow/).

## Twilio And Telecommunications

[Twilio](http://www.twilio.com) is a good example of a company using APIs to
make something that is normally really difficult very easy. Now you don't have
to worry about which cell phone network the number you are texting belongs to,
what country it is in, etc. Just integrate with the Twilio API, and you know
it's going to work.

For Twilio, **their API is their entire business**. There is no Twilio without
an API, because if Twilio was just a web form that sent a text message to any
given number - even if it still smoothed over all the carrier and location
differences - it would not acheive the goal of automation, and thus would
defeat the purpose.

In the years since Twilio launched, countless companies have relied on it for
things like [2-factor
authentication](http://en.wikipedia.org/wiki/Multi-factor_authentication) and
phone number verification via SMS. Twilio can even power your entire phone
system through tools like [OpenVBX](http://www.openvbx.org/), all with a
collection of REST APIs.

## The Bottom Line

If you don't have an open REST API that is easy to use, you will lose market
share to a competitor who does. It's time to start taking your API very
seriously. An API is a competitive advantage.

