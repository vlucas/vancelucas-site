---
title: 'Reading a FeedBurner Feed with PHP and cURL'
date: 2008-10-01
tags: curl, feedburner, php, php, xml
---

Just thought I'd post a quick HOW-TO article on how to get the contents of a
FeedBurner feed with PHP, because it's something I was attempting to do last
night that really annoyed me.  Since I started this blog here, I decided to
narrow another website of mine - [czaries.net](http://www.czaries.net) - to
just distribute some PHP scripts I've made and take down the news that was
there.  I replaced it with a short paragraph explanation and a feed of the
recent blog posts here.  The problem was, the feed wasn't displaying, and I
couldn't figure out why.

READMORE

I visited the feed URL in my browser, and viola - the XML feed content was
there.  I double-checked the URL in PHP, and it was the same, but I was getting
a 404 error instead.  Odd.  Turns out, viewing a FeedBurner feed requires the
presence of a USER-AGENT string (The string that identifies the operating
    system, browser, and language of your computer).  So the solution is to use
[PHP's cURL library](http://us2.php.net/manual/en/ref.curl.php) to send a
USER-AGENT string along with the request, like so:

```php
<?php
// URL location of your feed
$feedUrl = "http://feeds.feedburner.com/VanceLucas?format=xml";
$feedContent = "";
 
// Fetch feed from URL
$curl = curl_init();
curl_setopt($curl, CURLOPT_URL, $feedUrl);
curl_setopt($curl, CURLOPT_TIMEOUT, 3);
curl_setopt($curl, CURLOPT_FOLLOWLOCATION, true);
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
curl_setopt($curl, CURLOPT_HEADER, false);
 
// FeedBurner requires a proper USER-AGENT...
curl_setopt($curl, CURL_HTTP_VERSION_1_1, true);
curl_setopt($curl, CURLOPT_ENCODING, "gzip, deflate");
curl_setopt($curl, CURLOPT_USERAGENT, "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.9.0.3) Gecko/2008092417 Firefox/3.0.3");
 
$feedContent = curl_exec($curl);
curl_close($curl);
 
// Did we get feed content?
if($feedContent && !empty($feedContent)):
  $feedXml = @simplexml_load_string($feedContent);
  if($feedXml):
?>
  <h2>From The Blog...</h2>
  <ul>
    <?php foreach($feedXml->channel->item as $item): ?>
    <li style="padding:4px 0;"><a href="<?php echo $item->link; ?>"><?php echo $item->title; ?></a></li>
    <?php endforeach; ?>
  </ul>
  <?php endif; ?>
<?php endif; ?>
```

The code above gets the feed contents and then displays a nice summary of all
the headlines in an unordered list with links using
[SimpleXML](http://us2.php.net/manual/en/ref.simplexml.php).  Feel free to
replace the USER-AGENT string with your own, or simply leave it as-is.  The
current one is for Windows XP and Firefox 3.03.  You can get your own
USER-AGENT string from a `phpinfo()` call towards the bottom of the page in the
environment variables section.
