---
title: 'MySQL Error: 1033 Incorrect information in file'
date: 2011-03-02
tags: database, mysql, mysql, mysql-errors
---

I recently encountered this error on 
[Disposeamail](http://disposeamail.com) - a free disposable email site of mine that uses MySQL heavily for storing all incoming mail through an email pipe script.

I did a lot of researching, and basically, there are a few primary culprits I was able to identify that will hopefully save you some time.


more###Check your /tmp directory

MySQL will produce this error sometimes when the temp directory is not writeable.

*Ensure that 
**/tmp**
 (and/or /var/tmp) has the 
**correct permissions**
 (777)

	
*Check the 
**my.cnf**
file and search for a 
**tmpdir**
=/tmp flag. Ensure the value is pointing to the correct temp directory.

	
*Ensure your 
**/tmp directory is not full**

###Check your my.cnf


*If you 
**made changes**
 recently, 
**revert them**
 and restart MySQL (especially InnoDB Buffer Pool settings)

	
***Restore my.cnf.back**
 is there is one

	
*If you are using 
**InnoDB tables**
, ensure the 
**skip-innodb**
 line in my.cnf is 
**commented out**
 or removed.

###Clear InnoDB Log Files

This step 
**ONLY APPLIES IF THE ABOVE STEPS DID NOT WORK**
.

Read the 
[MySQL Manual page on removing InnoDB log files](http://dev.mysql.com/doc/refman/5.0/en/innodb-data-log-reconfiguration.html) for a safer backup and restoration procedures. Basically, the steps are:

*Shut down MySQL

	
***Remove ib_logfile***
 files from the MySQL data directory (move them or rename them if you want to be safe)

	
*Re-start MySQL
My specific problem was that somehow the "skip-innodb" line got added back into my "my.cnf" file, so MySQL was expecting a different table format when loading data. I suspect this had something to do with my cPanel/WHM setup overwriting the file, but I'll never know for sure.

Good Luck!