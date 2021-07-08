---
layout: single
title:  "Best practice for string format logging in python"
date:   2021-06-08 01:18:46 +0000
categories:
  - Python
---

In python3, f-string formating was introduced. It looks cool and convenient to use. The question is should we use f-string format in python logging.

Consider the 2 format styles with following example. We will measure the performance by processing 1000000 debug logging lines. The log level is ERROR, so the LogHandler won't take time to process them.
```
import logging
import time

logging.basicConfig()
log = logging.getLogger(__name__)
log.setLevel(logging.ERROR)

user = "Mario"

t1 = time.time()
for _ in range(1000000):
    log.debug("Hello %s!", user)

t2 = time.time()
for _ in range(1000000):
    log.debug(f"Hello {user}")

t3 = time.time()

print("Use % formatting took {:.3f} seconds".format(t2 - t1))
print("Use f-string formatting took {:.3f} seconds".format(t3 - t2))
```

And its result:

```
Use % formatting took 0.339 seconds
Use f-string formatting took 0.374 seconds
```

So the traditional % formating is the winner. The different here is because f-string formating took time to do the string formating before passing the string to debug() function, while % formatting just pass the raw string with args, the debug log is less severe than log level is set, log level ERROR, so it didn't take time to process the string formatting.

Wrap up:

- % string formatting: It has better performance.
- f-string formatting: It is more convenient.

Consider the pros and cons of them. :wink:
