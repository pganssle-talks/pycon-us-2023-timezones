# Concrete Time Zones: Local time

- UTC / Fixed Offsets <span style="color: green">Added in 3.2</span>
- Local time <span style="color: orange"><strong>○</strong> Basically supported in 3.6+</span>
- IANA Time Zones <span class="fragment" style="color: red" data-fragment-index="1">✘ (as of Python 3.8)</span>

<br/>
<br/>

Naïve datetimes are now considered system local times, and you can attach a fixed offset zone to them to probe time zone information:

```python
>>> print(datetime(2023, 11, 4, 12).astimezone())
2023-11-04 12:00:00-04:00
```

```python
>>> print(datetime(2023, 11, 5, 12).astimezone())
2023-11-05 12:00:00-05:00
```

<br/>

Setting `fold` on a naïve datetime works:

```python
>>> print(datetime(2023, 11, 5, 1, fold=0).astimezone())
2023-11-05 01:00:00-04:00
>>> print(datetime(2023, 11, 5, 1, fold=1).astimezone())
2023-11-05 01:00:00-05:00
```

<br/>

See my blog posts: [Why naïve times are local times](https://blog.ganssle.io/articles/2022/04/naive-local-datetimes.html) and [Stop using utcnow and utcfromtimestamp](https://blog.ganssle.io/articles/2019/11/utcnow.html)
