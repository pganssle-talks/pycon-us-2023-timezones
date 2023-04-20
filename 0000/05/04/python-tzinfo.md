# Python's Time Zone Model: `tzinfo`
<br/>

* Time zones are provided by *subclassing* `tzinfo`.
  <div style="height: 0.25em"></div>

* Information provided is a function of the datetime:
  <div style="height: 0.25em"></div>

    * `tzname`: The (usually abbreviated) name of the time zone at the given datetime
    * `utcoffset`: The offset from UTC at the given datetime
    * <span class="fragment disappearing-fragment nospace-fragment fade-out" data-fragment-index="1">`dst`: The size of the `datetime`'s DST offset (usually 0 or 1 hour)</span><span class="fragment nospace-fragment" data-fragment-index="1" style="color: #b70000"><strike>`dst`: The size of the `datetime`'s DST offset (usually 0 or 1 hour)</strike></span>

Notes:

So now that we've established that you actually need to *ugh* understand the abstractions you are working with, let's get into some of the details of how it works in Python.

Python's time zone model is based around an abstract base class `tzinfo`. The idea is that each time zone object provides three functions that take the `datetime` as an argument. There's `tzname`, which gives the name of the zone at the given datetime, `utcoffset`, which does most of the heavy lifting here, this gives the offset that applies at the relevant datetime, and then `dst`, which gives you the difference between the current offset and standard time. I think basically every time I've seen someone use the `dst` method it was something that I would consider a mistake, so basically never use that last one.
