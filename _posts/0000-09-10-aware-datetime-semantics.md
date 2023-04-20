# A curious case...

```python
>>> LON = ZoneInfo("Europe/London")

>>> x = datetime(2007, 3, 25, 1, 0, tzinfo=LON)
>>> ts = x.timestamp()
>>> y = datetime.fromtimestamp(ts, LON)
>>> z = datetime.fromtimestamp(ts, ZoneInfo.no_cache("Europe/London"))
```
<br/>

```python
>>> x == y
False
```
<fragment/>
<br/>


```python
>>> x == z
True
```
<fragment/>
<br/>

```python
>>> y == z
True
```
<fragment/>

--

# Hint

`2007-03-25 01:00:00` is imaginary in London!

```python
>>> print(x)                                # x (LON)
2007-03-25 01:00:00+01:00

>>> print(x.astimezone(timezone.utc))       # x (LON → UTC)
2007-03-25 00:00:00+00:00

>>> print(x.astimezone(timezone.utc).
...        .astimezone(LON))                # x (LON → UTC → LON)
2007-03-25 00:00:00+00:00
```

--


# What does equality mean?

<div style="display:flex; flex-flow: row nowrap; justify-content: space-between; align-items: flex-start; padding: 1rem;">

<div style="flex-grow: 2">
<ol>
    <li>Wall time semantics: compare only naïve portions<br/>
    <table style="margin-top: 0.5em">
        <tr>
            <td><tt>x == y</tt></td>
            <td><tt>False</tt></td>
        </tr>
        <tr>
            <td><tt>x == z</tt></td>
            <td><tt>False</tt></td>
        </tr>
        <tr>
            <td><tt>y == z</tt></td>
            <td><tt>True</tt></td>
        </tr>
    </table>
    <br/>
    </li>
    <li>Absolute time semantics: convert to UTC<br/>
    <table style="margin-top: 0.5em">
        <tr>
            <td><tt>x == y</tt></td>
            <td><tt>True</tt></td>
        </tr>
        <tr>
            <td><tt>x == z</tt></td>
            <td><tt>True</tt></td>
        </tr>
        <tr>
            <td><tt>y == z</tt></td>
            <td><tt>True</tt></td>
        </tr>
    </table>
    </li>
</ol>
</div>

<div style="border: 1px solid; padding-left: 5px; padding-right: 5px; background: #fff; align-items: flex-start">
<div class="fragment disappearing-fragment nospace-fragment fade-out" data-fragment-index="1">
<u>Wall Times:</u><br/>
<tt>
    x: <b>2007-03-25 01:00:00</b>+01:00<br/><br/>
    y: <b>2007-03-25 00:00:00</b>+00:00<br/><br/>
    z: <b>2007-03-25 00:00:00</b>+00:00<br/><br/>
</tt>
</div>
<div class = "fragment nospace-fragment fade-in " data-fragment-index="1">
<u>UTC:</u><br/>
<tt>
    x: 2007-03-25 <b>00:00:00+00:00</b><br/><br/>
    y: 2007-03-25 <b>00:00:00+00:00</b><br/><br/>
    z: 2007-03-25 <b>00:00:00+00:00</b><br/><br/>
</tt>
</div>
</div>
</div>

## Another hint <!-- .element: class="fragment" data-fragment-index="2" -->

```python
>>> x.tzinfo is y.tzinfo
True
```
<!-- .element: class="fragment" data-fragment-index="2" -->

```python
>>> x.tzinfo is z.tzinfo
False
```
<!-- .element: class="fragment" data-fragment-index="2" -->

--

# Semantics of aware datetime comparison:

1. When two `datetime`s are in the *same zone*, only the naïve portion is compared (wall time semantics).

2. When they are in *different zones*, both are converted to UTC first, then compared (absolute time semantics).

3. Two `datetime`s are in the "same zone" only if `dt1.tzinfo is dt2.tzinfo`.

<br/>
<br/>

## Mystery solved: <!-- .element: class="fragment" data-fragment-index="1" -->

<div class="fragment" data-fragment-index="1" style="text-align:center">
<table>
<tr>
    <td></td>
    <td>Wall</td>
    <td>Absolute</td>
    <td><tt>datetime</tt></td>
</tr>
<tr>
    <td><tt>x == y</tt></td>
    <td><b>False</b></td>
    <td>True</td>
    <td>False</td>
</tr>
<tr>
    <td><tt>x == z</tt></td>
    <td>False</td>
    <td><b>True</b></td>
    <td>True</td>
</tr>
<tr>
    <td><tt>y == z</tt></td>
    <td><b>True</b></td>
    <td>True</td>
    <td>True</td>
</tr>
</table>

</div>

--

# `zoneinfo`: Cache behavior

   Calls to the default constructor with identical arguments are guaranteed to return objects which compare as identical; specifically, the following must always be valid:

   ```python
   a = ZoneInfo(key)
   b = ZoneInfo(key)
   assert a is b
   ```

   This is because `datetime` assumes that time zones are singletons, which would cause confusing results if we used a simpler implementation:

   ```python
   >>> from datetime import *
   >>> from simple_zoneinfo import SimpleZoneInfo
   >>> dt0 = datetime(2020, 3, 8, tzinfo=SimpleZoneInfo("America/New_York"))
   >>> dt1 = dt0 + timedelta(1)
   >>> dt2 = dt1.replace(tzinfo=SimpleZoneInfo("America/New_York"))
   >>> dt2 == dt1
   True
   >>> print(dt2 - dt1)
   0:00:00
   >>> print(dt2 - dt0)
   23:00:00
   >>> print(dt1 - dt0)
   1 day, 0:00:00
   ```

See [PEP 615](https://www.python.org/dev/peps/pep-0615/) and [the documentation](https://docs.python.org/3/library/zoneinfo.html) for more information than you would ever want about working with the cache.
