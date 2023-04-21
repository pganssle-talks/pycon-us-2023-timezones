
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
