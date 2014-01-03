---
title: 'AppKernel - PHP Micro-Kernel'
date: 2009-09-28
tags: 
---

AppKernel is a lightweight service locator with some additional functionality and native extensibility. AppKernel was created to be a simple drop-in component for any project, with zero configuration.###Depend on AppKernel

AppKernel was created to solve a core problem that every object-oriented system faces: How to pass around dependencies to different parts of your application across multiple object instances, and how to access globally required information like configuration settings in a sane way. AppKernel solves this problem by becoming the sole dependency, and enabling everything else to be accessed from itself. AppKernel can be easily extended by adding custom methods dynamically to the already created object instance, or more traditionally through inheritance.

AppKernel can create and store objects, load class files, and profile your code. AppKernel also has two other primary features that every application needs - URL routing and a common way to access request data from multiple different sources.

###Core Responsibilities of AppKernel


*Configuration

	
*URL Routing

	
*Request Data Management

###Source Code / Download


**[http://github.com/vlucas/AppKernel](http://github.com/vlucas/AppKernel)**