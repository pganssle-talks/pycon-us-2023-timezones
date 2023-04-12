# Working with Time Zones: Everything You Wish You Didn't Need to Know (`zoneinfo` edition)

This is a talk to be given [at PyCon US 2023](https://us.pycon.org/2023/schedule/presentation/51/). The talk description is as follows:

Time zones are complicated, but they are a fact of engineering life. Time zones have skipped entire days and repeated others. There are time zones that switch to DST twice per year. But not necessarily every year. In Python it's even possible to create datetimes with non-transitive equality (a == b, b == c, a != c).

In this talk you'll learn about Python's time zone model and other concepts critical to avoiding datetime troubles. Using the zoneinfo module introduced in Python 3.9 (PEP 615), this talk covers how to deal with ambiguous and imaginary times, datetime arithmetic around a Daylight Savings Time transition, and datetime's new fold attribute, introduced in Python 3.6 (PEP 495).

External images used in this presentation can be found in the `external-images` folder, with an `attribution.md` file at each level of the folder giving attribution to the creators. If you notice any missing attributions, please report it as an issue.
