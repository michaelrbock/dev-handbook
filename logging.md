Logging
=======

Guidelines on logging

Log Levels
----------

# 1. Process Layer (ops)
e.g. containers, supervisor, buildpacks, etc

## Emergency (level 0)
The highest priority, usually reserved for catastrophic failures and reboot notices.

## Alert (level 1)
A serious failure in a key system.

Pagerduty alarms. Often bubbles up from error, warning, or critical logs.


# 2. Application Errors

## Critical (level 2)

Node- use 'console.error' *todo - use a logging library*
Python - use 'logging.error'
Go - 'log.Errorf' *todo - use a logging library*

Send to Sentry.

A failure in a key system.

Error happened, can't continue. The message right before exiting with code 1. See error handling docucumentation.

e.g "could not connect to database"

## Error (level 3)

Node- use 'console.error' *todo - use a logging library*
Python - use 'logging.error'
Go - 'log.Errorf' *todo - use a logging library*

Send to Sentry.

Something has failed. But the application can continue. See error handling documentation.

## Pattern
1. log error
   context (which function etc), message/reason, type user/external/internal, stack trace, (error code? used to find exact line where error was thrown, see error documentation)
   
2. send to sentry

# 3. Application Audit Information

Node- use 'console.log' *todo - use a logging library*
Python - use 'logging.info'
Go - 'log.Print(f|ln)' *todo -use a logging library*

## Warning (level 4)

Warning messages, not an error, but indication that an error will occur if action is not taken, e.g. file system 85% full - each item must be resolved within a given time.

Something is amiss and might fail if not corrected. 

Something bad happened that we recovered from, like "this shouldn't happen but it isn't fatal"


## Notice (level 5) (aka lifecycle")
Things of moderate interest to the user or administrator.

Auditing

e.g. "worker Y started with payload X"

progress information, significant events

# Pattern
Log starting and ending of major components

1. component name (e.g. which process/worker)
2. input or output
3. actor (user, process name, system id)
   
e.g. "system\_id=123456789012: csv-processor started with payload '123456789012'"
  "system\_id=123456789012: csv-processor successfully reserved 'reservation-123456789012'"
        
show the major work flow of our system
should be concise
should be filterable - if components go together, you should be able to filter for those components
should link related processes (e.g. using a prefix, using system-id in agents)


# 4. Application Status

## Info (level 6)

Node- use 'console.log' *todo - use a logging library*
Python - use 'logging.info'
Go - 'log.Print(f|ln)' *todo -use a logging library*

The lowest priority that you would normally log, and purely informational in nature.
Most common, how the application is running, what component is running at what point

e.g. "validating url"

mostly for investigative purposes after the fact or watching

# Pattern

1. Component name
2. Message/reason
3. Context (often input or env info)
  - name any object you dump (like payload, uri, csv headers, DOM object)

e.g. "csv-processor:info validating input payload '{"archive": ObjectId("123456789012")}'"

*** individual lines should be readable and provide any necessary context to understand what is going on at that point

avoid info logs within loops
don't clog up logs
filterable
concise

## Debug (level 7)

The lowest priority, and normally not logged except for messages from the kernel.

Often turned on or off based on environment.

Trace

Node - use 'debug'
Python - use 'logging.debug'
Go - no standard at the moment


# Pattern
none, whatever you need if its only for development environment, but remember your code is for other developers too, so make it clear why you are logging something or needed to log something


# Security

Don't log credentials (e.g. log usernames not username/password pairs) or sensitive information (e.g. student data)

Aim: Would it be ok to send these logs on hipchat or to a auditor? At least for notice and above.


- remember consecutive lines for one process are not necessarily consecutive in the logs with aggregation

- follow logging format to be set out below

- individual lines should be readable and provide any necessary context to understand what is going on at that point



TODO: logging format, machine parsable but still human readable
