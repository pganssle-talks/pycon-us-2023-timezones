# Introduction

## UTC

- Reference time zone
- Monotonic-ish (what's a few leap seconds between friends?)
<br/>
<br/>

## Time zones vs. Offsets <!-- .element: class="fragment" data-fragment-index="1" -->

<ul class="fragment" data-fragment-index="1">
    <li><tt>UTC-6</tt> is an offset</li>
    <li><tt>America/Chicago</tt> is a time zone</tt></li>
    <li><tt>CST</tt> is a highly context-dependent abbreviation:
        <ul>
            <li>Central Standard Time (<tt>UTC-6</tt>)</li>
            <li>Cuba Standard Time (<tt>UTC-5</tt>)</li>
            <li>China Standard Time (<tt>UTC+8</tt>)</li>
        </ul>
    </li>
<ul>

Notes:

We can start out easy, with UTC. UTC is the reference time zone, it's the 0 against which offsets are measured. It is mostly monotonic — which is to say that it doesn't have daylight saving time. It still has leap seconds, which is infuriating, since leap seconds don't belong in civil time *at all*, much less in the reference time zone, but there has recently been talk of ending the practice of leap seconds, so hopefully, going forward, UTC will actually record 1 second per second of elapsed time.

Another concept that is important to know is the difference between time zones and offsets.

UTC-6 is an offset. It means that you should add 6 hours to the local time to get a time in UTC.

`America/Chicago` is a time zone - it's a set of rules for what offsets and abbreviations apply in a given region as a function of time.

`CST`, which is the abbreviation for the offset that applies in Chicago right now, is a highly context-dependent abbreviation referring to an offset. In our context, it means Central Standard Time, but if you're in Cuba, it refers to Cuba Standard Time, and if you're in China, it refers to China standard time. Word of advice: never rely on these 3-letter abbreviations. Don't rely on them meaning a specific thing, and don't even rely on every zone having an associated 3-letter abbreviation.

[1m 15s; T: 1m 45s]
