---
title: 'IE6 and Multiple Button Submit Elements'
date: 2008-11-04
tags: browser-compatibility, general, ie, ie6, internet-explorer
---

I stumbled across yet another weird issue in Internet Explorer 6 today.  This time it has to do with <button type="submit"> elements and how data is sent back to the server.###<button type="submit"> vs <input type="submit">

You would think both elements would behave exactly the same, because they're both basically the same thing - the "submit" button on HTML forms.  Turns out that in Internet Explorer 6, they don't behave the same at all.  Surprise, surprise.  The issue has to do with using multiple submit buttons on a form which can perform different actions:

[Displayed cart contents with editable quantity fields]

Update

Checkout >

more
The standard behavior of forms is to only pass the submit button field that was clicked by the user.  So if the user updated some quantities and clicked on "Update", only that <button type="submit"> would be posted back to the server, and the "checkout" field wouldn't even be set.  So the code on the server to check which button was clicked might look something like this on the server:

// Which button was clicked?
if(isset($_POST['update'])) {
    // Update cart quantities
} else { 
    // Save cart and redirect to checkout page
}

###What's Going On?! @#%$ IE!

Turns out, IE6 actually passes ALL <button type="submit"> elements back to the server on postback instead of just the active/clicked one.  As a result, the "update" element will ALWAYS be set for ie6 users, and the "else" condition will NEVER be executed.  They won't be able to make it past the cart page, and won't be able to buy your products or continue to the next step.

###The Solution

So the solution here is to change all the <button type="submit"> elements to <input type="submit"> elements instead, because those are actually handled correctly in IE6.

[Displayed cart contents with editable quantity fields]

So much for the flexibility of being able to use HTML markup inside <button> elements.