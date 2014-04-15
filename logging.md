Logging
=======

Logging is _TODO: define logging_

The purpose of logging is to allow transparency into computer programs. It is used to keep tabs on processes running and to allow investigative analysis. Some examples of when logs are investigated include: audits, crash reports, partner dialogs, certain aggregation metrics, and diagnosing problems.

Logging can focus on a particular part of code (unit) or on a whole process (integration).

Logs are broken into severity levels. Each level has a set of standards. These levels are grouped into four larger categories that we are more concerned with:
    1. Process Layer
    2. Application Errors
    3. Application Audit Information
    4. Application Statuses

1. Process Layer (ops)
----------------------

e.g. containers, process control systems, buildpacks, etc

These logs are usually dealt with in dev-ops environments. We give a brief summary of these levels.

### Emergency (level 0)
*Emergency: system is unusable*
The highest priority, usually reserved for catastrophic failures and reboot notices.

### Alert (level 1)
*Alert: action must be taken immediately*
A serious failure in a key system.

These are the Pagerduty alarms. They often bubbles up from error, warning, or critical logs.


2. Application Errors
---------------------

### Critical (level 2)
A failure in a key system.

This is when an error occurred and the process cannot recover and continue. It is often logged right before exiting with code 1. See error handling documentation for best error handling practices.

e.g "could not connect to database"


### Error (level 3)
Something has failed.

This is when an error occurred but we recovered and the application can continue running. See error handling documentation for best error handling practices.

e.g. "requested page does not exists"

******************************************

### Logging practices
1. Log error

**Node**: use 'console.error' *todo - use a logging library*

**Python**: use 'logging.error'

**Go**: 'log.Errorf' *todo - use a logging library*

2. Send to Sentry or appropriate error alert system.


### Logging Format

1. error context (which function etc)
2. message/reason
3. type user/external/internal
4. stack trace
5. (error code? used to find exact line where error was thrown, see error documentation)


3. Application Audit Information
--------------------------------

### Warning (level 4)
Something is amiss and might fail if not corrected.

These are warning messages (not an error) but an indication that an error will occur if action is not taken.

e.g. "file system 85% full"

### Notice (level 5) (aka "lifecycle")
Things of moderate interest to the user or administrator.

This is information such as progress information, significant events, auditing information.

e.g. "worker Y started with payload X"

******************************************

### Pattern

Log starting and ending of major components or anything needed to show the major work flow of our system.

**Node**: use 'console.log' *todo - use a logging library*

**Python**: use 'logging.info'

**Go**: 'log.Print(f|ln)' *todo - use a logging library*


### Format

1. component name (e.g. which process/worker)
2. input or output
3. actor (user, process name, system id)

e.g. "system\_id=123456789012: csv-processor started with payload '123456789012'"
  "system\_id=123456789012: csv-processor successfully reserved 'reservation-123456789012'"


4. Application Status
---------------------

### Info (level 6)

The lowest priority that you would normally log, and purely informational in nature.

This is the most common type of logging. It explains how the application is running, what component is running at what point, etc. It is mostly for investigative purposes after the fact.

e.g. "validating url"


#### Pattern

Log starting and ending of major components or anything needed to show the major work flow of our system.

**Node**: use 'console.log' *todo - use a logging library*

**Python**: use 'logging.info'

**Go**: 'log.Print(f|ln)' *todo - use a logging library*


#### Format

1. Component name
2. Message/reason
3. Context (often input or env info)
  - *name any object you dump (like payload, uri, csv headers, DOM object)*

e.g. "csv-processor:info validating input payload '{"archive": ObjectId("123456789012")}'"

**NOTE** individual lines should be readable and provide any necessary context to understand what is going on at that point.

**NOTE** avoid info logs within loops


### Debug (level 7)

This is the lowest priority, and normally not logged except for messages from the kernel. It is often turned on or off based on the environment. It is essentially a trace.

#### Pattern

**Node**: use 'debug'

**Python**: use 'logging.debug'

**Go**: no standard at the moment


##### Format
There is no standard format. Debugging is for whatever you need as it is only for the development environment.

**NOTE** your code is for developers too, so make it clear why you are logging something or needed to log something.


Security
--------

Don't log credentials.
  e.g. log usernames not username/password pairs) or sensitive information (e.g. student data)

**NOTE** Would it be ok to send these logs on hipchat or to a auditor? This is a must for notice level and above.


Best Practices
--------------
**Individual lines should be *readable* and provide any necessary *context* to understand what is going on at that point.**

- remember consecutive lines for one process are not necessarily consecutive in the logs with aggregation

- Be concise

- log lines should be filterable - if components go together, you should be able to filter for those components

- log messages should link related processes (e.g. using a prefix, using system-id in agents)


- follow logging format to be set out below



TODO: logging format, machine parsable but still human readable
