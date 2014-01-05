---
title: 'Early Performance Benchmarking is a Disease'
date: 2008-10-18
tags: benchmark, benchmarking, performance, php, php, profiling
---

Benchmarking and performance concerns should be one of the last things you
address while building your application, but it seems as though, in the PHP
community especially, it's often one of the first things novice developers
think about.

Any PHP developer who's been in the community for a while [has
heard](http://reinholdweber.com/?p=3) preposterous claims like "use single
quotes (') for strings instead of double quotes ("), because it's faster". 
That is, faster over the 100,000 or so iterations it took the tester to
generate a number sufficiently large enough to justify the claim, with a
particular version of PHP, in a particular development environment in which
it was tested.

READMORE

These articles are then followed up with
[other](http://www.chazzuka.com/blog/?p=163)
[posts](http://blogs.digitss.com/php/php-performance-improvement-tips/) that
only serve to perpetuate and solidify the original and invalid performance
claims.  Articles like these are like a plague to the PHP community, spreading
around and steering novice developers in the wrong direction, concerning them
with the wrong things.  Chris Vincent recently released
[PHPBench.com](http://www.phpbench.com/) - a website that benchmarks chunks of
related PHP code against each other to compare the speeds.  His goal was just a
simple discovery of what code is actually fastest, and although it does yield a
few interesting results, I have to wonder if it even matters.  If the tests
prove anything at all, it's that the specific syntax you choose doesn't
matter.  Why? Because it shows that newer PHP versions (he runs it on PHP 5.2+
  where most of the previous articles were based on PHP 4.x) have been
corrected and optimized to fix any performance discrepencies that there may
have been in the past.  In the "double (") vs. single (') quotes" test, Chris
concludes:>"In today's versions of PHP it looks like this argument has been
satisfied on both sides of the line. Lets all join together in harmony in this
one!"

Similarly, the differences in the "echo vs. print" and "for vs. while" tests
are so close in speed (and again remember, this is over 1,000 iterations) that
you're just better off using which ever one is a better fit the for job you're
doing.

### Optimize for Performance AFTER You Code

The true way to optimize your code to run faster is to worry about optimization
after you code.  This is, of course, because the realbottlenecks in your code
can only be identified after the code is already written.  When put into
simplified steps, the development and optimization process would then look
something like this:

1. Write code
2. Run it for a while until you (or your users) start to experience noticeable slowdowns or you begin to reach the capacity of the hardware you're running on
3. [Profile](http://www.sitepoint.com/blogs/2007/04/23/faster-php-apps-profile-your-code-with-xdebug/)
  [your](http://devzone.zend.com/article/2899-Profiling-PHP-Applications-With-xdebug)
  [code](http://www.pseudocoder.com/archives/2007/04/24/how-to-really-use-xdebug-to-speed-up-your-app/)
  to identify where the realbottlenecks are
4. Re-factor and make adjustments to your code to fix those bottlenecks
5. Go back to step 2

Now that's not to say that performance concerns should be completely discarded
when writing your code - experienced developers will automatically make code
and design decisions based on their experience that will have a beneficial
impact on performance while they're writing the code.  This just means that it
should never be your primaryfocus when developing your application, and that
you should never be overly concerned about it until it's actually a problem.

### The Impact on Your Code

So the real difference here is that when you listen to benchmarks and have
performance as a high priority immediately, you end up making bad design
decisions and waste time worrying and doing things that may not even affect
performance at all in the end.  So instead, focus on your users, and do what
you can to make their lives easier, no matter what you think ahead of time the
performance impact may be.  If it becomes a problem in the future, you can turn
your focus to the specific problem and fix your code where the problems
actually are.

### Conclusion

Put aside your worries about performance and **just build it**.  Would you
rather have 10,000 users using a website with performance problems that you
have to fix or 100 users using a website that is half as useful but runs 10%
faster because you spent all your time focusing on performance "tips" when they
may not even have an impact on performance at all?

