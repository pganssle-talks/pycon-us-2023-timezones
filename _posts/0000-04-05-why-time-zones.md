# Why do we need to work with time zones at all?
<br/>

```python
from dateutil import rrule as rr
from datetime import datetime, timezone
from zoneinfo import ZoneInfo

# Close of business in New York on weekdays
closing_times = rr.rrule(freq=rr.DAILY, byweekday=(rr.MO, rr.TU, rr.WE, rr.TH, rr.FR),
                         byhour=17, dtstart=datetime(2023, 3, 8, 17), count=5)

NYC = ZoneInfo("America/New_York")
for dt in closing_times:
    print(dt.replace(tzinfo=NYC))
```
<pre style="margin-top: 0.5em">
2023-03-08 17:00:00-05:00
2023-03-09 17:00:00-05:00
2023-03-10 17:00:00<b>-05:00</b>
2023-03-13 17:00:00<b>-04:00</b>
2023-03-14 17:00:00-04:00
</pre>
<br/>

```python
# Get close of business in UTC
for dt in closing_times:
    print(dt.replace(tzinfo=NYC).astimezone(timezone.utc))
```
<pre style="margin-top: 0.5em">
2020-03-05 22:00:00+00:00
2020-03-06 22:00:00+00:00
2020-03-09 21:00:00+00:00
2020-03-10 21:00:00+00:00
2020-03-11 21:00:00+00:00
<pre>

--

<div style="position: fixed; display: flex; align-items: center; justify-content: space-around; top: 40%; font-size: 3rem">
<div style="max-width: 80%">
When storing datetimes where the <em>wall time</em> matters (e.g. meetings), you must store local time, because the mapping between UTC and local time is <em>not stable</em>.</div>
</div>

--

<p style="text-align: center">
<img src="images/lebanon_news.png" alt="Lebanon wakes up in two time zones because of daylight savings spat — a news article from reuters about Lebanon changing their time zone at the last minute."
/>

</p>

<p style="text-align: center">
<img src="images/timing_of_timezone_changes.png" alt="A screenshot of the opening paragraph of Matt Johnson-Pint's 'On the Timing of Time Zone Changes'"
/>
</p>

<span><em>Further Reading:</em> 
<a href="https://codeofmatt.com/on-the-timing-of-time-zone-changes/">On the Timing of Time Zone Changes</a>, <a href="https://codeofmatt.com/time-zone-chaos-inevitable-in-egypt/">Time Zone Chaos Inevitable in Egypt</a></span>
