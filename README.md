# Datetime Coordinates

---

This python package implements a feature allowing easy coding of datetime-like data without the
annoyances of handling datetime-like data. Assim

It provides:

* high level function decorator `datetime_coords`
* lower level class `DateTimeCoord` for building other more customized decorators.
* a marker `DatetimeLike` to indicate which data should be seen as datetime-like.

---

## Installation

Within your python enviroment type

> `pip install datetimecoords`

## Usage

This simplest way of using it is applying directly the decorator into a function wherein datetime-like data is
been processed.

There is only a short protocol to be followed:

*  datetime-like parameters must be argument in a function (whether positional or keyword) and be typehinted with the special marker `DatetimeLike` provided by the package.

* function must be defined as generic dictionary kws arguments or the special reserved keys: `_unit`; `_ref` or both.
  * valid units for `_unit` parameter are the ones supported by numpy datetime dtype ([numpy datetime units](https://numpy.org/doc/stable/reference/arrays.datetime.html#datetime-units))
  * `_ref` should be datetime-like and represents the "zeroth" location on the coordinates to which the data is been referenced under the hood.

* As of now, nesting `Datetimelike` in `Sequence` or similar, won't do no good.

## Quickstart

Let's have a basic function computing velocity as an average speed. The function should
implement the following reasoning:

> $$ v_{avg} = (x_2 - x_1) / (t_2 - t_1) $$

Start off by importing *basic objects*

```
from dtcoords import datetime_coords, DatetimeLike
```

They are meant to be used together.

```
@datetime_coords
def compute_average_seed(t1: DatetimeLike, x1: float, t2: DatetimeLike, x2: float, **kwargs) -> float:
    delta_x = x2 - x1
    delta t = t2 - t1
    return delta_x / delta_t
```

Note that `t1` and `t2` are supposed to be datetime-like as a pandas Timestamp or the builtin python datetime,
nonetheless within the code they can be regarded as just common numeric data. Thus the code becomes more aligned
with how human brain thinks regarding time as quantity. So, regular operations are readily used and the return number is an ordinary `float`.

The function could then be called within a script as

```
import datetime
t1 = datetime.datetime(...)
t2 = datetime.datetime(...)
x1: float = ...
x2: float = ...
```

if x's are given in meters (for instance), then we are determinig our average speed in meters per seconds by requiring computation to be run with time unit in seconds `s`.

```
avg_speed = compute_average_speed(t1, x1, t2, x2, _unit='s')
```

---

**autor: Conrado**
