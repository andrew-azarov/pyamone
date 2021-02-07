# pyamone

A long time ago our python cronjobs required to ensure only one script/job is running.
Solutions at the time like tendo.singleton had minor bugs which made it incompatible with OSs we use.
We reworked the singleton solution to make sure it works in any environment in our company.
This module supports atomic file locks on FreeBSD.
Windows/Linux has no support for atomic file locks however we used the fcntl locks in this case.
Of course you have a minimal possibility of duplicate run within ~0.000017 of a second.
In the future we might switch to semaphores and mutexes. However they have their own drawbacks for example when you have zombie processes.

This module provides an OnlyOne() class which atomically creates a lock file containing PID of the running process to prevent parallel execution of the same program.

In case there is any other instance running already an `OnlyOneException` will be thrown.

The class will throw `IOError` and `OSError` in case there are hardware or OS level corruption.

    >>> from pyamone import OnlyOne
    ... me = OnlyOne(filename="test.lock", path="/tmp")

or
    
    ... with OnlyOne() as lock:
    ...     example()
    
This is helpful for both daemons and simple crontab scripts. Works on *NIX and Windows OS's.

By default this creates a lock file with a filename based on the program name.
In case you want to remove the file or finish the lock beforehand just delete the instance in script.

TODO:

- Implement atomic mutexes for Windows
- Implement atomic semaphores for POSIX.