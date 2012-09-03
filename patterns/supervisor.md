---
title: "Putting RQ under supervisor"
layout: patterns
---

## Putting RQ under supervisor

Supervisor is a popular tool for managing long-running processes in production
environments.  It can automatically restart any crashed processes, and you gain
a single dashboard for all of the running processes that make up your product.

RQ can be used in combination with supervisor easily.  You'd typically want to
use the following supervisor settings:

{% highlight ini %}
[program:myworker]
; Point the command to the specific rqworker command you want to run.
; If you use virtualenv, be sure to point it to
; /path/to/virtualenv/bin/rqworker
; Also, you probably want to include a settings module to configure this
; worker.  For more info on that, see http://python-rq.org/docs/workers/
command=/path/to/rqworker -c mysettings high normal low
process_name=%(program_name)s

; If you want to run more than one worker instance, increase this
numprocs=1

; This is the directory from which RQ is ran. Be sure to point this to the
; directory where your source code is importable from
directory=/path/to

; RQ requires the TERM signal to perform a warm shutdown. If RQ does not die
; within 10 seconds, supervisor will forcefully kill it
stopsignal=TERM

; These are up to you
autostart=true
autorestart=true
{% endhighlight %}