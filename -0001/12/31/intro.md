<h1 style="font-size: 3.5em">Working With Time Zones</h1>
<br/>
<h2 style="font-size: 2.5em">Everything You Wish You Didn't Need to Know</h2>
<h2 style="font-size: 2.5em">(<tt>zoneinfo</tt> Edition)</h2>
<br/>
<br/>
<br/>
<span style="font-size: 2.5em">
Paul Ganssle
</span>
<br/>
<br/>
<img src="images/pganssle-logos.svg" height="40px" alt="@pganssle">
<br/>
<br/>
<span style="font-size: 1em;"><em>This talk on Github:
<a href="https://github.com/pganssle-talks/pycon-us-2023-timezones">pganssle-talks/pycon-us-2023-timezones</a></em>
</span>
<br/>
<a rel="license" href="https://creativecommons.org/publicdomain/zero/1.0/">
    <img src="external-images/logos/cc-zero.svg" height="45px">
</a>
<br/>

Notes:

Four years ago, I came to PyCon and gave a talk called Working with Time Zones: Everything You Wish You Didn't Need to Know. That same year, I gave a talk at the language summit called "Time Zones in the Standard Library", where I proposed adding a new time zone provider into the standard library, which eventually led to the creation of the `zoneinfo` module, mostly but not completely obsoleting my old talk. So this year I decided for the second edition of my time zone talk.

OK, so we've got a lot to cover and only 30 minutes to cover it in, so let's jump right in with the basics of time zones!

(30s)
