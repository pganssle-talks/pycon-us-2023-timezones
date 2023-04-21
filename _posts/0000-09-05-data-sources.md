
# data source: IANA Time Zones

- Provides historical time zone information
- Standard open source (public domain) source for time zone information
- Shipped with many operating systems
- Source for `dateutil` and `pytz`'s data.
- 2-21 releases per year (average 9)

<br/>
<div style="text-align: center">
<img src="images/all_zones.png" alt="Map of IANA time zones"/>
</div>

Notes:

So I've mentioned IANA time zones a few times, and that refers to the source of the data. This is also sometimes called the Olson database or tzdb. This is basically the canonical time zone database that pretty much everyone uses. It's got historical time zone information going back to at least 1970, and in most places far longer than that.

It's open source and shipped on many operating systems, but the problem is that it cuts releases between 2 and 21 times per year, sometimes on very short notice. On average, this happens more frequently than there are months with 30 days in them, which doesn't exactly work well with Python's yearly release cadence, much less the frequency at which most deployments of Python get updated, so we cannot tie the data directly to Python.

(IANA = Internet Assigned Numbers Authority)

--

# PEP 615: Support for the IANA Time Zone Database in the Standard Library

<div style="text-align:center">
<img 
    style="max-height:45vh"
    src="images/zoneinfo-documentation.png"
    alt="A screenshot of Python 3.9's zoneinfo documentation."/>
</div>

- When the system has IANA time zone data available, it is used
    - Defaults to well-known deployment locations
    - Configurable in-program using `zoneinfo.reset_tzpath`
    - Configurable with environment variable `PYTHONTZPATH`
    - Default can be set at compile time

- We also provide the `tzdata` package on PyPI — a "first party" data-only fallback library.

Notes:

The solution we decided on for this conundrum was to make this basically a "bring your own data" situation. `zoneinfo` will look for the IANA data in well-known locations, and the search path is configurable a number of ways. We also provide a PyPI package that you can depend on as a fallback for platforms that don't ship the IANA data in an accessible way, like Windows.

--

# How to use `zoneinfo`

## Backport (3.6+)

```python
try:
    from backports import zoneinfo
except ImportError:
    import zoneinfo
```
<br/>

## Construct aware datetime

```python
>>> dt = datetime(2020, 11, 12, 19, tzinfo=ZoneInfo("America/Chicago"))
>>> print(dt)
2020-11-12 19:00:00-06:00

>>> print(datetime.now(ZoneInfo("America/Chicago")))
2020-11-12 19:46:21.211438-06:00
```
<br/>

## Convert between zones

```python
>>> print(dt.astimezone(ZoneInfo("Europe/Paris")))
2020-11-13 02:00:00+01:00
```

Notes:

So now if you are using any sort of supported version of Python, you can use the `zoneinfo` module for your IANA zones. In versions earlier than 3.9, there's a backport you can use, but however you get your `ZoneInfo` objects, you can now safely use the standard Python idioms for constructing aware datetimes as you'd see it in the documentation: passing the `tzinfo` to the constructor, to `.replace` or `.now` or `.astimezone`.

However, it is worth noting that if you are migrating from `pytz`, it turns out that you are increasingly likely to encounter the somewhat counter-intuitive semantics built in to Python's `datetime` module.
