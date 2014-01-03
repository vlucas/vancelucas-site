---
title: 'Protected vs Private Scope: Arrogance, Fear, and Handcuffs'
date: 2011-04-04
tags: method-scope, oop, opinion, php, php, private-vs-protected, programming
---

The age old private vs protected debate has been re-ignited in the PHP
community recently following the decision of [Doctrine2 and Symfony2 to make
all class methods
private](http://groups.google.com/group/symfony-devs/browse_thread/thread/58a0d015622c13cb)
until there is a very clear and proven reason to change them to protected or
public. The intention is a good one - to ensure they are providing a clear and
stable API through intentional and known extension points that they can better
test and support. Fabien (the creator of Symfony) points to this benefit in an
[article of his
own](http://fabien.potencier.org/article/47/pragmatism-over-theory-protected-vs-private)
explaining the thinking behind the decision. The primary started point of
Fabien's article and the driving thought behind this whole change and
philosophy is (emphasis his):
  > **Having a few well defined extension points force the developer to extend your library the right way instead of hacking your code.**

The problem is that this kind of thinking is a slippery slope that kills the
spirit of programming. It alienates the more pragmatic developers within
communities. Telling other developers that you are going to force them to work
with your code in some pre-determined "right way" is an incredibly arrogant
statement to make.  more Instead of letting the codebase grow organically and
be modified or extended at will, you handcuff developers and send them a
different message. Allow me to re-phrase it using my own words:

>I know the right way to use this code and I have thought of all the possible
>use cases. If you don't agree, then you have to prove me wrong and wait until
>I either agree or figure out a better right way for you.

This is the real message developers hear when internal code is private scope or
marked with the final keyword - a message that they have arbitrarily limited
power and ability to make changes where they need to. But it doesn't stop
there.

When you take the position that all methods should be private unless proven
otherwise, you plant a seed of fear in the developers that use your code. Fear
that whenever they may have to veer off the beaten path to meet a project
requirement they won't be able to. Fear that they might miss deadlines waiting
for the "right way"  to enable the functionality they need. Fear that they
might have to modify or hack the actual file or class they need to change the
behavior of to achieve the functionality they want within the project deadline
because they are not able to extend it.

Using private scope removes the fun from programming and kills the hacking
spirit by creating fear through the knowledge that you will not have the
ability to make changes anywhere in the codebase you need to when you need to.
You are no longer in control of your own code. You were forced to submit to a
[nanny state
mentality](http://blog.astrumfutura.com/2011/03/private-vs-protected-methods-the-debate-that-never-ends/)
by sacrificing your freedoms to modify the code for the promise of future
stability and safety. That's not a fun place to be when you're on the receiving
end, and it's not a policy I would ever consider enforcing with my own code. I
sincerely hope this line of thinking does not proliferate within the PHP
community and infect other projects.

