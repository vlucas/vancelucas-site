---
title: 'NoSQL First Impressions: Object Databases Missed the Boat'
date: 2010-04-27
tags: database, databases, mongodb, nosql, nosql, opinion, programming, rdbms-oodb
---

I've spent the past few weeks here at work researching and playing with 
[NoSQL](http://en.wikipedia.org/wiki/NoSQL) databases (and especially 
[MongoDB](http://mongodb.org)) for a new feature we're developing that doesn't easily fit into a relational model. And so far, I 
really like what I see. The profoundness of the shift away from the relational model and the implications that has just blow my mind. 
**You no longer have to fragment your data to persist it**
. You just store it. That's it. No more hours toiling over the design your table schema and how to break apart the data you put together just to fit it into a relational model. You can now store your data exactly how you use it in your application, with no other special needs or 
[impedance mismatches](http://en.wikipedia.org/wiki/Object-relational_impedance_mismatch). Going back to an 
[RDBMS](http://en.wikipedia.org/wiki/RDBMS) system now just seems illogical - it's like breaking apart a camera tripod just to fit it in the same standard size case you've been using for years instead of just collapsing it and finding a different case that fits it better. All that effort you go though tearing down your tripod and putting it back together every time you use it is wasted and unnecessary. It's a symptom of the larger problem that your case doesn't fit your tripod.
more

Once you fully get the NoSQL viewpoint and the implications it has on  the future of data storage, you have one of those "why didn't anyone  think of this sooner?" moments. And yes, I know key/value stores like 
[memcached](http://memcached.org/) existed long before this whole new "NoSQL" movement, but it's not quite  the same thing.###Object Databases

Object databases are an interesting case. They are in the "NoSQL" category and solve the same problem other NoSQL databases do - creating a storage mechanism that fits your data instead of making your data fit a predefined rigid storage mechanism. So why didn't they catch on? What gives? It's anyone's guess why object databases didn't catch on and achieve widespread adoption, but one thing is for sure - 
**object databases really missed the boat**
 while other NoSQL database technologies are running rings around them.

The primary problem is that most object databases are designed for a specific language, like Java or .NET. Some have more language support than others, but the problem is still the same - by integrating in directly with a language's object model they depend on specific language implementations, which severely limits adoption. They solve the impedance mismatch problem that RDBMSes present, but 
**fail to provide language-agnostic data persistence and access**
, effectively tying their use to specific technology stacks and platforms.

###The Best of Both Worlds

NoSQL databases are gaining traction like no other database technology has in over 30 years. The reason is that 
**good NoSQL databases solve both key data storage problems: impedance mismatches AND language-agnosticism.**
 By storing the data in an intermediary format like JSON (or BSON) and allowing custom file content types, the focus is on the actual data itself instead of specific technologies or data models. The technology stack becomes irrelevant, and the walls to gaining user adoption crumble.

The bottom line is that NoSQL databases are not just the latest fad. They represent a fundamental paradigm shift in data storage and are an entirely new way of thinking about how to store and access your data in a way that makes sense for your actual usage. NoSQL databases are absolutely something you should be paying attention to and following closely - they are a strong contender to the RDBMS model and will likely become the de-facto data storage choice for most new web applications within a decade.