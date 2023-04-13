# History of Python's Time Zones

When `datetime` was introduced in [Python 2.3](https://docs.python.org/3/whatsnew/2.3.html#date-time-type), there were *no* concrete time zones in the standard library.

```python
from dateutil import relativedelta as rd  # Cheating...

class ET(tzinfo):
    def utcoffset(self, dt):
        if self.isdaylight(dt):
            return timedelta(hours=-4)
        else:
            return timedelta(hours=-5)

    def dst(self, dt):
        if self.isdaylight(dt):
            return timedelta(hours=1)
        else:
            return timedelta(hours=0)

    def tzname(self, dt):
        return "EDT" if self.isdaylight(dt) else "EST"

    def isdaylight(self, dt):
        dst_start = datetime(dt.year, 1, 1) + rd.relativedelta(month=3, weekday=rd.SU(+2),
                                                               hour=2)
        dst_end = datetime(dt.year, 1, 1) + rd.relativedelta(month=11, weekday=rd.SU,
                                                             hour=2)

        return dst_start <= dt.replace(tzinfo=None) < dst_end
```

--

# History of Python's Time Zones: Concrete Time Zones

- UTC / Fixed Offsets
- Local time
- IANA Time Zones

--

<!-- .slide: data-visibility="hidden" -->

# History of Python's Time Zones: Ambiguous time problem

```python
EASTERN = ET()

print(datetime(2017, 11, 4, 12, 0, tzinfo=EASTERN))
print(datetime(2017, 11, 5, 12, 0, tzinfo=EASTERN))
```

```
2017-11-04 12:00:00-04:00
2017-11-05 12:00:00-05:00
```

<br/><br/>

```python
dt_before_utc = datetime(2017, 11, 5, 0, 30, tzinfo=EASTERN).astimezone(datetime.UTC)
dt_during = (dt_before_utc + timedelta(hours=1)).astimezone(EASTERN)  # 1:30 EDT
dt_after = (dt_before_utc + timedelta(hours=2)).astimezone(EASTERN)   # 1:30 EST

print(dt_during)   # Lookin good!
print(dt_after)    # OH NO!
```

```
2017-11-05 01:30:00-04:00
2017-11-05 02:30:00-05:00
```

--

# History of Python's Time Zones: Ambiguous time problem
<br/>

Ambiguous times are times where the same "wall time" occurs twice, such as during a DST to STD transition.

<br/>

```python
from dateutil import tz

dt1 = datetime(2004, 10, 31, 4, 30, tzinfo=timezone.utc)
for i in range(4):
    dt = (dt1 + timedelta(hours=i)).astimezone(NYC)
    print('{} | {} |  {}'.format(dt, dt.tzname(), 
                                   'Ambiguous' if tz.datetime_ambiguous(dt)
                                   else 'Unambiguous'))
```

<br/>
<pre>
2004-10-31 00:30:00-04:00 | EDT |  Unambiguous
2004-10-31 01:30:00-04:00 | EDT |  Ambiguous
2004-10-31 01:30:00-05:00 | EST |  Ambiguous
2004-10-31 02:30:00-05:00 | EST |  Unambiguous
</pre>

<br/>

There can be multiple times in a time zone differentiated by their offset!

--

# History of Python's Time Zones: Imaginary times

The complement of ambiguous times is imaginary times — wall times that don't exist in a given time zone, such as during an STD to DST transition.


```python
dt1 = datetime(2004, 4, 4, 6, 30, tzinfo=timezone.utc)
for i in range(3):
    dt = (dt1 + timedelta(hours=i)).astimezone(NYC)
    print(f'{dt} | {dt.tzname()} ')
```

<br/>
<pre>
2004-04-04 01:30:00-05:00 | EST
2004-04-04 03:30:00-04:00 | EDT
2004-04-04 04:30:00-04:00 | EDT
</pre>

Notice the lack of a `2004-04-04 02:30:00`!

--

# History of Python's Time Zones: Concrete Time Zones

- UTC / Fixed Offsets <span class="fragment" style="color: green" data-fragment-index="1">✔ Added in 3.2</span>
- Local time
- IANA Time Zones

<p style="text-align: center">
<img src="images/whatsnew3.2.png" alt="What's new in Python 3.2 excerpt"
     class="fragment" data-fragment-index="1" />
</p>
