---
title: 'Getting Calendar Queries Right: Event Dates in SQL'
date: 2012-04-24
tags: general
published: false
---

The problem of retrieving events within a certain date range sounds like an easy one, and it is in some cases. But the problem I see all too often is when programmers try to get a list of events with start and end dates that happen within a certain date range - say one month. A total of four different dates are in play in this scenario - the start and end date for the event, and the start and end date of the date range you are searching in (usually the first to the last day of the month). For many, the problem seems easy at first. The query ends up looking something like this:SELECT *
FROM events
WHERE date_start BETWEEN '2009-08-01' AND '2009-08-31'
    OR date_end BETWEEN '2009-08-01' AND '2009-08-31'
And that's usually as far as it goes. The code is called done, and when put into production, people wonder why there are events that don't show up that they know for a fact should be showing up. The problem is, the simple query above only accounts for three possible scenarios of date ranges. There are four.

###The Four Different Date Range Scenarios

There are four different, unique scenarios that occur that all need to be accounted for in order to always list all possible dates correctly, for any given date range:

***Inside**
: The event 
starts and ends within the date range

	
***Start Inside**
: The event 
starts within and 
ends after the date range

	
***End Inside**
: The event 
starts before and 
ends within the date range

	
***Outside**
: The event 
starts before and 
ends after the date range (spans over)
This concept might be better expressed as an image:


[![](http://www.vancelucas.com/wp-content/uploads/2010/03/four-date-range-scenarios.png)](http://www.vancelucas.com/wp-content/uploads/2010/03/four-date-range-scenarios.png)
