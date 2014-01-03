---
title: 'Listing Aliases Inside an Android Keystore File With Keytool'
date: 2010-12-29
tags: android, android-mobile, android-sdk, general, keystore, keystore-alias, keytool, mobile
---

If you lose or forget your Android keystore file alias that is used to build
APK files for distribution (like I did when trying to package [Autoridge Lite](http://autoridge.com/mobile)
for the Android Market), here is a quick and easy way to see them:

1. Open a Terminal Window, Run This Command:

  ```
  keytool -list -keystore /location/of/your/com.example.keystore
  ```

  Make sure "keytool" is either in your `PATH`, or "cd" into the "tools" directory where your Android SDK files are.
2. Enter your keystore password when prompted (you didn't forget that too, did you? Did you?)
3. See results!
You should see something like the picture below if you did everything right.
The alias is circled in yellow. If you have multiple aliases in your keystore,
they will all be listed, one per line.

READMORE

[![Google Android List Keystore Aliases](http://www.vancelucas.com/wp-content/uploads/2010/12/android-keystore-list-aliases.png)](http://www.vancelucas.com/wp-content/uploads/2010/12/android-keystore-list-aliases.png)

And since you already forgot your keystore alias, I might as well remind you to
put your keystore in a safe location so you don't lose it. Remember - if you
lose your keystore, you're screwed. You won't be able to release any updates
for the apps you packaged with it. I put mine in my
[Dropbox](https://www.dropbox.com/referrals/NTEwNTczODY5) for safe keeping. It
is synced to three of my personal computers and backed up to an external USB
drive in addition to being stored remotely on the Dropbox servers.

