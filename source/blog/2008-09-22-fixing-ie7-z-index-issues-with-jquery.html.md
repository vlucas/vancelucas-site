---
title: 'Fixing IE7 Z-Index Issues with jQuery'
date: 2008-09-22
tags: javascript, javascript, jquery, jquery
---

For some reason, Internet Explorer 7 does some pretty funky things, and has 
[several known bugs](http://www.quirksmode.org/bugreports/archives/explorer_7/index.html) with it's rendering engine that drive web developers like me crazy.  While most of the known bugs occur in relatively obscure situations and go largely unnoticed, there are a few that really stick out and cause web developers to waste many hours trying to fix them.  The way IE7 renders z-index stacking orders is one of them.

One way to fix many of the issues with IE7 is to dynamically reverse the default z-index stacking order of the elements on your page. This will ensure the elements higher in your HTML source will also have a higher z-index order on your page, solving most of the IE stacking issues.
more If you're using 
[jQuery](http://jquery.com) (the best Javascript library there is), here's the quick fix:


$(function() {
	var zIndexNumber = 1000;
	$('div').each(function() {
		$(this).css('zIndex', zIndexNumber);
		zIndexNumber -= 10;
	});
});

This code will start with a z-index of 1000, and decrement the z-index for each DIV element of the page by 10, giving the first DIV a z-index of 1000, the second, 990, the third 980, and so on. Notice that the selector will find all DIV elements with the code "$('div')", using the same syntax as CSS selectors.  If your HTML code has different requirements, feel free to change the code or the selector to suit your needs by following jQuery's 
[documentation on selectors](http://docs.jquery.com/Selectors).###Update for MooTools (04/14/2009):



[A generous commenter](http://www.liamsmart.co.uk/) has posted the code for fixing z-index issues with 
[MooTools](http://mootools.net/) 1.2:


if(Browser.Engine.trident){
	var zIndexNumber = 1000;
	$$('div').each(function(el,i){
		el.setStyle('z-index',zIndexNumber);
		zIndexNumber -= 10;
	});
};