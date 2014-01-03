---
title: 'Mirror Remote Server With Linux FTP Commands'
date: 2009-02-18
tags: backup, command-line, data-backup, ftp, general, linux
---

A client of mine called me last night around 8:00pm a little worried. I had recently setup a hosting account for her on my server, so that she would be able to switch from her current FTP-only solution to a full hosting account with a domain and everything for when she makes a webstie in the future (she only needs the FTP to share files for now).  On the phone, she said:>The guy who hosts my files just called me.  He got in a disagreement with the guy who manages his servers, and told me to back everything up because it might not be there tomorrow


Wow. In a split second, all your data can be gone. 
The forever kind of gone. The problem was - and the reason she called me - was that she had amassed so many files over the years, that it would take days to backup using her internet connection, and she only had hours to get it done.  Okay, relax, I told her - I've got it taken care of.  I can use linux shell commands to download all the files to my server from yours.  It will be much faster, and the files will go directly to the new server instead of having to be re-uploaded there, saving some very time-consuming steps.

Okay, I thought. I'll just login, make a big tarball of all the files and grab that with my server. But her file hosting account did not allow shell access, and probably didn't have the extra space for an additional tarball of all the files anyway. So I'm stuck with the linux ftp commands - or so I thought.  Turns out, the 
mget ftp command does not recursively download folders on most servers. So the best function to use on a remote linux server that you can't run shell commands on is 
wget, because 
wget also supports the FTP protocol. The usage goes like this:


wget -r ftp://user:pass@domain.com

That was going fine, and then the connection was cut-off by the remote server a short way through getting all the files, probably due to some data transfer cap or something. I re-started it, and it cut off again near the same place. So this isn't enough either, and I still don't want to do it manually. Luckily, there is a wget flag to ignore already existing files - '-nc'. So the whole command to download everything recursively and 
not re-download files you already have is:


wget -nc -r ftp://user:pass@domain.com

Remember to back up often. You never know when you might find yourself in a sudden and unexpected data loss situation, 
[like Ma.Gnolia did](http://blog.wired.com/business/2009/01/magnolia-suffer.html) Friday, January 30th. There's a good discussion happening on the 
[SitePoint open thread on data loss](http://www.sitepoint.com/blogs/2009/01/31/open-thread-how-to-prevent-data-loss/) that same some good backup ideas and methods, too.