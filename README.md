# Things that will eventually bite you

There are many, many things that can bite you as a software engineer who works
on dsitributed systems. I've decided to start collecting them.

## Category: missing limit

We often get into trouble because we don't bound some value, and at some point
i's much greater than we ever expected.

Also, for many APIs, it's simpler to not specify a limit. When the wrong
thing is easier than the right thing, the wrong thing is going to happen.

### Unbounded queue

Queues that can grow without limit can lead to resource exhaustion. [Jeff
Hodges](https://www.somethingsimilar.com/about) mentioned this in a great talk
he dd at RICON West 2013 entitled "Practicalities of Productionizing
Distributed Systems" which unfortunately has been removed from the Internet.

### Missing timeout on a network call

Eventually, a network call you make is going to block "forever".

### Incorrect database query whose scope was too great

The equivalent of `select * from ...`.

Ad-hoc queries are one of the most dangerous queries of this type, because those are user-initiated.


## Category: resource exhaustion

Fred Brooks once said: "The programmer, like the poet, works only slightly
removed from pure thought-stuff. He builds his castles in the air, from air,
creating by exertion of the imagination."

The problem is that software runs on actual, physical machines, and those
machines have finite resources, and those resources can be exhausted. 

The obvious ones are CPU, memory, and disk space, but there are others.

### Out of file descriptors

You can run out of file descriptors if you create too many files. On Linux,
the resulting error message is, confusingly, "No space left on device".


## Category: incorrect time calculations

You'll inevitably need to do calculations on time data (e.g., seconds, minutes,
hours, days, months, years). There are tons of corner cases in these types of
calcluations.

### Time zone calculations

See [the.endoftimezones.com] by [@aaronblohowiak].

[the.endoftimezones.com]: http://the.endoftimezones.com/

[@aaronblohowiak]: https://twitter.com/aaronblohowiak


