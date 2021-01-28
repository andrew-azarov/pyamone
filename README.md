# pyamone

A long time ago we required to ensure only one script or job is running.
Solutions at the time like tendo.singleton had minor bugs which made it incompatible with OSs we use.
I reworked the singleton solution to make sure it works in any environment in our company.

This module provides an OnlyOne() class which atomically creates a lock file containing PID of the running process to prevent parallel execution of the same program.

In case there is any other instance running already an `OnlyOneException` will be thrown.

The class will throw `IOError` and `OSError` in case there are hardware or OS level corruption.

    >>> from pyamone import OnlyOne
    ... me = OnlyOne(filename="test.lock", path="/tmp")

    
This is helpful for both daemons and simple crontab scripts. Works on *NIX and Windows OS's.

By default this creates a lock file with a filename based on the program name.
In case you want to remove the file or finish the lock beforehand just delete the instance in script.
