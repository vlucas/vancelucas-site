---
title: 'Handling Exceptions in Gearman Tasks (Even Background Ones)'
date: 2012-07-31
tags: background-jobs, gearman, php, php, programming, workers
---

I recently had some issues with Gearman tasks throwing exceptions and killing
the whole Gearman daemon. This made it nearly impossible to trace errors back
to their origin, because the logged exception stack trace didn't provide much
useful information, because it just logged where it failed in Gearman - not the
actual file and line of code that was doing the work. I dug into the code and
started trying things like
[GearmanClient::setExceptionCallback](http://us.php.net/manual/en/gearmanclient.setexceptioncallback.php)
and running the tasks, but since the tasks were being run with
[addTaskBackground](http://us.php.net/manual/en/gearmanclient.addtaskbackground.php)
instead of just
[addTask](http://us.php.net/manual/en/gearmanclient.addtask.php), the callbacks
were never getting executed, and I still was not able to do anything to handle
exceptions for the jobs that were being run (and they were still killing the
    Gearman daemon). Clearly, I was going to have to get a little more
creative.

The only other place to add code that will catch exceptions for all jobs run is
in the GearmanWorker::addFunction method. So I looked at the following
one-liner for adding named job callbacks:

```php
<?php
$worker->addFunction($name, $task);
```

And replaced it with a closure that uses a try/catch and then logs any
exceptions to [Exceptional](http://www.exceptional.io/) so we can see the full
stack trace and exact point of failure for any job - even background jobs:

```php
<?php
$worker->addFunction($name, function() use($task) {
	try {
		$result = call_user_func_array($task, func_get_args());
	} catch(\Exception $e) {
		$result = GEARMAN_WORK_EXCEPTION;
		echo "Gearman: CAUGHT EXCEPTION: " . $e->getMessage();

		// Send exception to Exceptional so it can be logged with details
		Exceptional::handle_exception($e, FALSE);
	}

	return $result;
});
```

And it works beautifully. Now all the jobs are run, the Gearman daemon is never
killed by a PHP process, and all the exceptions are logged with full granular
details that makes it easy to troubleshoot and fix any errors.
