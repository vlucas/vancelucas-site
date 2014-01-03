---
title: 'jQuery UI Datepicker with AJAX and LiveQuery'
date: 2009-06-03
tags: ajax, javascript, jquery, jquery, jquery-ui, livequery
---

I've been a little aggravated lately trying to get 
[jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) to work correctly on dynamically added fields for creating additional line items to invoices for 
[InvoiceMore](http://www.invoicemore.com). It works great for fields already displayed on the page, but it tends to have major issues with dynamically added fields through 
[AJAX or AHAH](http://microformats.org/blog/2005/12/18/ajax-vs-ahah/). Of course it won't work out of the box with elements added dynamically to the DOM, so we can use 
[jQuery's $.live() event](http://docs.jquery.com/Events/live) (new in 1.3 - you previously had to use 
[liveQuery](http://docs.jquery.com/Plugins/livequery)) to make it work. The Datepicker works by binding to the focus() event by default, but as of jQuery 1.3.2, the 'focus' event cannot be monitored by the 'live' event function. So we're stuck with a little work around:

You would think just a simple "$(this).datepicker()" call wrapped inside the live() event would work, but it doesn't. Turns out that in order to get it working consistently, you have to add the 'showOn: focus' config option as well as manually focusing on the element with the focus() event. Charming.