---
title: 'Easier Titanium XHR and AJAX Requests'
date: 2011-09-15
tags: mobile, mobile-app, titanium, titanium-2
---

One question I see a lot on the
[Appcelerator Titanium Developer Q&A](http://developer.appcelerator.com/questions) is how to perform AJAX requests and/or work with APIs, etc. There is a built-in way to do this with the
[Ti.Network.HTTPClient](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.Network.HTTPClient-object.html) module that is pretty easy, but it does have a few drawbacks and "gotchas", like executing the "success" event for ANY returned status code - even 500 errors. Since working with APIs is so common with mobile apps, I made a wrapper function modeled after
[jQuery's $.ajax method](http://api.jquery.com/jQuery.ajax/) that I use in all my apps. It shortens the syntax quite a bit and is much more familiar to those who are used to using jQuery.

The usage looks like this:

```javascript
// Geocode input text location
app.utils.ajax({
    url: 'http://maps.googleapis.com/maps/api/geocode/json?address=' + txtLocation.value +'&region=us&sensor=true',
    method: 'get',
    success: function(xhr) {
        var data = JSON.parse(xhr.responseText);
        Ti.API.info(data);

        if("OK" == data.status) {
            var res = data.results[0];
            if(res) {
                alert("Location: " + res.geometry.location.lat + ', ' + res.geometry.location.lng);
            }
            // Do something with coordinates
        } else {
            Ti.UI.createAlertDialog({
                title: 'Geocode Error',
                message: 'Unable to geocode location input'
            }).show();
        }
    },
    error: function(xhr) {
        Ti.UI.createAlertDialog({
            title: 'Geocode Error',
            message: 'No location matches found. Please try something else.'
        }).show();
    }
});
```


And the actual code for the utility function:

```javascript
/**
 * Application Utilities and Helper Methods
 **/
(function(_app) {
    _app.utils = {};

    // AJAX method that mimmicks jQuery's
    _app.utils.ajax = function(_props) {

        // Merge with default props
        var o = _app.combine({
            method: 'GET',
            url: null,
            data: false,
            contentType: 'application/json',

            // Ti API Options
            async: true,
            autoEncodeUrl: true,

            // Callbacks
            success: null,
            error: null,
            beforeSend: null,
            complete: null
        }, _props);

        Ti.API.info("XHR " + o.method + ": \n'" + o.url + "'...");
        var xhr = Ti.Network.createHTTPClient({
            autoEncodeUrl: o.autoEncodeUrl,
            async: o.async
        });

        // URL
        xhr.open(o.method, o.url);

        // Request header
        xhr.setRequestHeader('Content-Type', o.contentType);

        if(o.beforeSend) {
            o.beforeSend(xhr);
        }

        // Errors
        xhr.setTimeout(10000);
        xhr.onerror = function() {
            Ti.API.info('XHR "onerror" ['+this.status+']: '+this.responseText+'');
            if(null !== o.error) {
                return o.error(this);
            }
        };

        // Success
        xhr.onload = function() {
            // Log
            Ti.API.info('XHR "onload" ['+this.status+']: '+this.responseText+'');

            // Success = 1xx or 2xx (3xx = redirect)
            if(this.status < 400) {
                try {
                    if(null !== o.success) {
                        return o.success(this);
                    }
                } catch(e) {
                    Ti.API.info('XHR success function threw Exception: ' + e + '');
                    return;
                }
            // Error = 4xx or 5xx
            } else {
                Ti.API.info('XHR error ['+this.status+']: '+this.responseText+'');
                if(null !== o.error) {
                    return o.error(this);
                }
            }
        };

        // Send
        if(o.data) {
            Ti.API.info(o.data);
            xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
            xhr.send(o.data);
        } else {
            xhr.send();
        }

        // Completed
        if(null !== o.complete) {
            return o.complete(this);
        }
    };

    // And it does depend on this code below to combine object properties:

    // Extend an object with the properties from another
    // (thanks Dojo - http://docs.dojocampus.org/dojo/mixin)
    var empty = {};
    function mixin(/*Object*/ target, /*Object*/ source){
        var name, s, i;
        for(name in source){
            s = source[name];
            if(!(name in target) || (target[name] !== s && (!(name in empty) || empty[name] !== s))){
                target[name] = s;
            }
        }
        return target; // Object
    };
    _app.mixin = function(/*Object*/ obj, /*Object...*/ props){
        if(!obj){ obj = {}; }
        for(var i=1, l=arguments.length; i<l; i++){
            mixin(obj, arguments[i]);
        }
        return obj; // Object
    };

    // Create a new object, combining the properties of the passed objects with the last arguments having
    // priority over the first ones
    _app.combine = function(/*Object*/ obj, /*Object...*/ props) {
        var newObj = {};
        for(var i=0, l=arguments.length; i<l; i++){
            mixin(newObj, arguments[i]);
        }
        return newObj;
    };
})(app);
```

The particular organization of this utilities file assumes that your app
structure follows the organization model shown in the [Tweetanium example
app](https://github.com/appcelerator-titans/tweetanium) (using the 'app'
namespace within a single window context), but is easy to adapt if you are not.
