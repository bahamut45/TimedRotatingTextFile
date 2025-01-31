# timed-rotating-text-file

[![made-with-python](https://img.shields.io/badge/Made%20with-Python-1f425f.svg)](https://www.python.org/) [![PyPI license](https://img.shields.io/pypi/l/timed-rotating-text-file.svg)](https://pypi.python.org/pypi/timed-rotating-text-file/) [![PyPI version shields.io](https://img.shields.io/pypi/v/timed-rotating-text-file.svg)](https://pypi.python.org/pypi/timed-rotating-text-file/) [![PyPI pyversions](https://img.shields.io/pypi/pyversions/timed-rotating-text-file.svg)](https://pypi.python.org/pypi/timed-rotating-text-file/) 
[![GitHub release](https://img.shields.io/github/release/bahamut45/timed-rotating-text-file.svg)](https://GitHub.com/bahamut45/timed-rotating-text-file/releases/)

A ZERO dependency rotating text file handler which rotates when YOU want it to, like TimedRotatingFileHandler provided by Python's logging module.

### When to use this module?
- When you have that niche case of logs being written into some text file from several functions and you just want it to rotate without having to re-write all the write calls with some new package.

### Why not use TimedRotatingFileHandler provided by Python's logging module?
- If you are thinking of logging, please use TimedRotatingFileHandler. It provides doRollover method which does the same functionality but with all the logging APIs.


### Installation
`pip install timed-rotating-text-file`

### Usage

If you have some code like:
```python
with open("path/to/text/file","w") as fp:
  fp.write("some line")
```
All you have to do is:
```python
from timed_rotating_text_file import TimedRotatingTextFile
with TimedRotatingTextFile("/tmp/tmp.log", when="m", backup_count=5) as fp:
  fp.write("some log")
``` 

By default, the interval is 1 so in our example, the rotation will be done every minute.

Result:

```bash
-rw-r--r-- 1 root root  840 avril 12 21:03 /tmp/tmp.log
-rw-r--r-- 1 root root 1,7K avril 12 20:53 /tmp/tmp.log.2021-04-12_20-51
-rw-r--r-- 1 root root  840 avril 12 20:55 /tmp/tmp.log.2021-04-12_20-53
-rw-r--r-- 1 root root  840 avril 12 20:57 /tmp/tmp.log.2021-04-12_20-55
-rw-r--r-- 1 root root 1,7K avril 12 20:59 /tmp/tmp.log.2021-04-12_20-57
-rw-r--r-- 1 root root  840 avril 12 21:01 /tmp/tmp.log.2021-04-12_20-59
```

### Arguments

`class TimedRotatingTextFile(filename, when='d', interval=1, backup_count=0, mode="ab+", delay=False, utc=False)`

Returns a new instance of the `TimedRotatingTextFile` class. The specified file is opened and used as the stream for file. On rotating it also sets the filename suffix. Rotating happens based on the product of **when** and **interval**.

You can use the **when** to specify the type of **interval**. The list of possible values is below. Note that they are not case sensitive.

| Value      | Type of Interval      |
|------------|-----------------------|
| 'M'        | Minutes               |
| 'H"        | Hours                 |
| 'D'        | Days                  |
| 'MIDNIGHT' | Roll over at midnight |

The system will save old files by appending extensions to the filename. The extensions are date-and-time based, using the strftime format `%Y-%m-%d_%H-%M-%S` or a leading portion thereof, depending on the rollover interval.

When computing the next rollover time for the first time (when the handler is created), the last modification time of an existing file, or else the current time, is used to compute when the next rotation will occur.

If the **utc** argument is true, times in UTC will be used; otherwise local time is used.

If **backup_count** is nonzero, at most **backup_count** files will be kept, and if more would be created when rollover occurs, the oldest one is deleted. The deletion logic uses the interval to determine which files to delete, so changing the interval may leave old files lying around.

If **delay** is true, then file opening is deferred until the first call

> Calculation of the initial rollover time is done when the handler is initialised. Calculation of subsequent rollover times is done only when rollover occurs, and rollover occurs only when emitting output. If this is not kept in mind, it might lead to some confusion. For example, if an interval of “every minute” is set, that does not mean you will always see files with times (in the filename) separated by a minute; if, during application execution, output is generated more frequently than once a minute, then you can expect to see files with times separated by a minute. If, on the other hand, messages are only output once every five minutes (say), then there will be gaps in the file times corresponding to the minutes where no output (and hence no rollover) occurred.

Inspired by :
- https://docs.python.org/3/library/logging.handlers.html#timedrotatingfilehandler
- https://github.com/Rahul-RB/RotatingTextFile
