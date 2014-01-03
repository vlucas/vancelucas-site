---
title: 'Zero to App in Two Weeks with Titanium'
date: 2010-12-27
tags: android, appcelerator, autoridge, cross-platform, iphone, mobile, mobile-app, programming, titanium, titanium-2
---

Like any web developer who has been sitting on the sidelines watching this 
[mobile explosion](http://www.readwriteweb.com/archives/admob_reports_on_mobile_webs_explosive_growth.php) happen in front of my eyes, I was eager to find a way to jump in. Up until about a month ago, I was still evaluating various different mobile development platforms - 
[Titanium](http://www.appcelerator.com/), 
[PhoneGap](http://www.phonegap.com/), and 
[Rhomobile](http://rhomobile.com/), trying to decide which one I wanted to use to make my first mobile app. My only selection criteria was that the platform would have to be able to produce both iPhone and Android apps with the same code, because I wanted to be able to develop and release both an Android and iPhone apps without having to learn two completely different programming languages and development environments, and without having to do everything twice.
more

After playing with all three platforms, I settled on 
[Appcelerator Titanium](http://www.appcelerator.com/) as my tool of choice for a few main reasons:*It produces native code, and is not just a simple html/css "web view" wrapper

	
*It is javascript-based, which means I don't have to learn a new language or try wrap my head around entirely new design paradigms

	
*The application code is UI and event focused, which seems to make the most sense for native apps ("draw this button here", "do this on click", etc)

###Starting Out

Starting your first project with Titanium is a bit rough. There are a ton of steps involved in setting up the Android development environment, and you're pretty much on your own on how to do this from Appcelerator's standpoint - the Android Developer website has 
[instructions](http://developer.android.com/sdk/installing.html) on how to do this, and it's pretty much expected that you have gotten at least that far before beginning with Titanium. Xcode is easy to setup in comparison, but it's a painfully long download at over 3GB.

Note that you're going to have to have these environments setup regardless of the platform you choose, so it's really not any discredit to the Titanium platform that they don't have guides for this, although I think it would be wise for them to make some for the new-to-mobile-development people like me.

From a platform and code standpoint, the main thing that makes starting an Appcelerator project a bit tough is that there is little to no clear direction - the development process is literally wide open for you to do anything you want to do. This is good because it provides flexibility, but harder for newcomers because there is no defined answer for basic questions like "how do I organize my files?" or "where do I put configuration?". It's like a programming language without a framework. The answer is almost always "wherever you want to". You try a few things, find what works for you, and then copy that structure and base skeleton files for all your future projects. What helped me most here is looking at a few existing open-source projects (like 
[this one](https://github.com/givp/Titanium-Mobile-Reference-Directory-App)) and figuring out what they did.

###The Development Experience

After you settle on an organization structure and get your app actually building and running an an emulator, the rest of the process is pretty easy and fairly smooth. The code syntax is calls to custom APIs with basic Javascript and JSON notation for options/settings, so most web developers will feel right at home, especially if they have ever had experience with javascript, jQuery or DOM scripting.

If you're approaching Titanium from an MVC perspective, working with Titanium will be like spending most of your time in the "View" layer, and there really is no controller. Titanium and most native applications tend to be event-driven, and it works well with Javascript syntax and the way they have it setup. You're essentially making direct calls within a script file, telling Z window or button to render at X,Y with given display properties.

Want a new blank window to open?

var myWin = Ti.UI.createWindow({
    title: 'New Window',
    url: 'mydir/new_window.js'
});
myWin.open();
Want a button that scrolls an image slideshow to the previous image on click?

// Create button object
var btnLeft = Ti.UI.createButton({
        left: 10,
        width: 30,
        height: 30,
        backgroundColor: '#333',
        image:'../images/icon_arrow_left.png'
    });

// Add button to current window
win.add(btnLeft);

// Scroll to previous image on click/touch
btnLeft.addEventListener('click', function(e) {
        if (activeViewIndex == 0) {
            return;
        }
        activeViewIndex--;
        scrollView.scrollToView(activeViewIndex);
    });
Most other basic operations are similarly easy and trivial to accomplish with Titanium. The code is fairly straight forward, is easy to understand, and does what it says. Furthermore, there really is no setup or boilerplate code that you have to write at all, which means once you get the hang of Titanium, you can create apps pretty quickly.

###Cross-Platform Mobile Development

If your wish was to find a development platform that smooths out all the idiosyncrasies and differences between all the different mobile platforms and provide you with a single way to achieve the same things on all platforms, Titanium may not be for you.

Most of the provided Titanium UI APIs will work cross-platform, but they are tied to the native platform implementations of those features, so they are not guaranteed to look, work, or even act the same cross-platform. This leads to a lot of "try it and see" type development that consists of switching back and forth between code, re-building the project, and seeing how it works in the emulator on on your device. Although this type of blind development can be pretty frustrating at times, this approach makes sense and is actually a benefit to me as a sole developer. It means I don't have to spend any time at all on styling, UI, images, or look and feel of the app - I just get the native look and feel out of the box. This is also good from an end-user standpoint, because users will have the same familiar look and feel with your app as they have with most others they use. Contrast this with development tools like PhoneGap where you use HTML/CSS, and you're stuck creating your own whole look and feel with CSS (or using something like 
[jQuery Mobile](http://jquerymobile.com/)) for each app, and having the same look for all mobile platforms. That could be beneficial in some cases, but to an end user, suddenly having an iOS look on an Android app you launch is a bit odd when all the other apps you use have the Android UI.

###The Difficult Pieces

The most difficult thing to deal with in Titanium is when a particular feature I want to use is well-supported on the iPhone, but causes immediate crashes in Android, with no warning from the compiler that the feature I am trying to use is not supported on the Android platform. This has lead to quite a bit more time-consuming "try it and see if it works" type development than I had originally bargained for -- and the Android emulator isn't exactly quick to re-compile and re-launch an app after making changes.

These developer headaches have not gone unheard through - Appcelerator just released 
[Titanium Mobile SDK 1.5.0](http://developer.appcelerator.com/blog/2010/12/titanium-mobile-1-5-0-is-ga-today.html), which focused on full Android support. I upgraded and re-compiled a few of my apps to the new SDK, and most of the issues have been smoothed out. I have even been able to use code I had previously removed and have it work flawlessly now, so kudos to the Appcelerator Android team on their hard work. They are obviously committed to the platform and making it the best they can for developers that work on it.


###The Finished Product

The app I made ended up being fairly simple - It's an automotive app for 
[Autoridge](http://autoridge.com) and the goal is to view vehicle photos by vehicle year/make/model, and view part numbers by category. It doubles as a simplistic vehicle database. The goal is to provide select part information for common replacement parts for your vehicle when you need to buy a part, but the part catalog is missing or your vehicle is not represented.

From a technical standpoint it's pretty simple as well - It communicates with a JSON REST API and just displays the results on-screen in a native table view, with little special styling and no animations. The photo gallery was the most difficult and time-consuming piece of the whole app, and the 1.5 SDK release provided 
much better support for it on Android (even built-in swipe gestures for photo transitions!).


[![Autoridge Lite for the iPhone](http://autoridge.com/assets/images/mobileapp/iphone/ss1-320.png)](http://autoridge.com/mobile) 
[![Autoridge Lite for the Android](http://autoridge.com/assets/images/mobileapp/android/ss1-320.png)](http://autoridge.com/mobile)


###Summary

Overall, I have grown pretty fond of Appcelerator's Titanium platform. It allows me to create iOS and Android apps quickly and without having to learn any new programming languages - a feat I had previously thought was out of my reach. The platform is in a noticeably beta-like status (currently at Mobile SDK v1.5.1 at the time of writing) with frequent updates and even some truly frustrating compatibility-breaking changes and inexplicably missing functionality. If you're willing to stick with Titanium through the rough spots, you will be able to outpace native developers both in terms of line-for-line productivity and to-market speed for 
simple most apps, and will be able to compile them for both iOS and Android platforms as an added bonus.