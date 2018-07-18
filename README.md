# Things that will eventually bite you as a back-end software engineer

There are many, many things that can bite you as a software engineer who works
on distributed systems. I've decided to start collecting them.

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

### Out of inodes

You can run out of inodes if you create too many files. On Linux,
the resulting error message is, confusingly, "No space left on device".

## Category: incorrect time calculations

You'll inevitably need to do calculations on time data (e.g., seconds, minutes,
hours, days, months, years). There are tons of corner cases in these types of
calcluations.

### Time zone calculations

Time zone calculations are invariably horrible (Do you know your UTC offset?)

The motivation for see [the.endoftimezones.com] by [@aaronblohowiak].

[the.endoftimezones.com]: http://the.endoftimezones.com/

[@aaronblohowiak]: https://twitter.com/aaronblohowiak

## Category: Things not running where you think they will

Credit to [@aaronblohowiak] for these.

### Dev or test talks to prod

### Code not intended for prod runs in prod

## Category: Miscellaneous interactions

### Synchronized clients

Clients become synchronized in their interactions with a server. For example, a
service that provides leases goes down, and then comes back up, and all of the
clients get a lease that expires at the same time, and so they all renew at the
same time.

Synchronized retries is another example.


Typically, if you see code that uses jitter in a timeout or lease expirationi
time, it's because somebody got bit by this.

### Implicit contract

At some point, you're going to replace a function/module/service with something which
you think implements the same API, yet still causes things to break. There's
some implicit assumption that's no longer true with the replacement thing.

See also [Hyrum's Law][hyrum]

[hyrum]: http://www.hyrumslaw.com/

## Category: Errors and failures

### Incorrect exception handling

Exception-handling code paths are much less likely to be exercised, either in
automated tests, manual tests, or even when the service has been in production
for a while.

This means that there are all sorts of opportunities for incorrect behavior
when an uncommon error happened that results in an exception.

For more details, see: [Simple Testing Can Prevent Most Critical Failures: An Analysis of Production Failures in Distributed Data-Intensive Systems][exception-paper]
by Yuan et al.

[exception-paper]: https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf

### Gray failures

Sometimes systems undergo partial failures where the system isn't working quite
right (e.g., it's very slow), but it's still behaving well enough that none of
your alerts have triggered to notify you that this is an issue.

See [Gray Failure: THe Achilles' Heel of Cloud-Scale Systems][gray-failure] by
Huang et al.

[gray-failure]: https://www.cs.jhu.edu/~huang/paper/grayfailure-hotos17.pdf



