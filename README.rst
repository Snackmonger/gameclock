=================
Custom Game Clock
=================

.. contents::

Installation
------------

``py -m pip install custom_gameclock``


Description
-----------
The custom game clock package provides a simple, customizable clock/calendar for use in a
video game. The clock is controlled by calling the ``GameClock().tick()`` method,
and the user is responsible for ensuring that the method is called at an appropriate interval and with correct timing.

Usage
-----
The clock will keep time based on user-defined calendar systems, which are wrapped in a CalendarFormatting object. This allows the clock to track
calendars with different month and day names, as well as different numbers of days, months, hours, etc. compared to the English Gregorian calendar.

Simple, easy, out-of-the-box
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the user doesn't need a customized calendar, then ``GameClock()`` will return a calendar formatted to the English Gregorian system. 

.. code:: Python

    import time
    from custom_gameclock import GameClock, Speeds

    clock = GameClock()
    running = True
    while running:
        clock.tick()
        time.sleep(Speeds.NORMAL)

This example uses the standard library ``time``, but the user can customize the main loop in whatever
way is desired.

Display
~~~~~~~

Nicely-formatted time is returned from the ``GameClock().prettytime`` property, and
a plain dictionary timestamp is returned from the ``GameClock().current_time`` property:

.. code:: Python
    >>> from custom_gameclock import GameClock
    >>> x = GameClock()
    >>> x.prettytime
    '00:00 - Sunday, January 1, 1'
    >>> x.current_time
    {'minutes': 0, 'hours': 0, 'day_of_week': 'sunday', 'month': 'january', 'day_of_month': 1, 'year': 1, 'leap_year': 0}

Custom Calendars
~~~~~~~~~~~~~~~~

Custom calendar systems are supported by passing an instance of the ``CalendarFormatting`` class when initializing
the clock.

Days and Months
+++++++++++++++
The ``Days`` and ``Months`` enums are used to define the names of the days and months that the calendar will use.
Any names can be used, as long as they are unique in their enum.

CalendarFormatting
++++++++++++++++++
The ``CalendarFormatting`` class is initialized with a dictionary of limits, as well as the ``Days`` and ``Months`` enums.
This dictionary defines the points at which different units of time will roll over into the next unit.
The class checks that the names of the months are the same as those in the ``Months`` enum, and that the leap month is a valid name.

.. code:: Python
    from enum import auto
    from gameclock import GameClock, Days, Months, CalendarFormatting

    values = {'leap_month': 'winter', 
              'leap_year_frequency': 3, 
              'minutes_in_hour': 100, 
              'hours_in_day': 14, 
              'days_in_month': {'spring': 28, 
                               'summer': 28, 
                               'fall': 28, 
                               'winter': 28}
                }
    class FantasyGameMonths(Months):
        SPRING = auto()
        SUMMER = auto()
        FALL = auto()
        WINTER = auto()

    class FantasyGameDays(Days):
        MORDOCH = auto()
        KELLENCRAT = auto()
        DRAGGENTHAR = auto()

    cal = CalendarFormatting(values, FantasyGameDays, FantasyGameMonths)

    starting_time = {'minutes': 66, 
                     'hours': 12, 
                     'year': 33, 
                     'month': 'winter', 
                     'day_of_month': 24, 
                     'day_of_week': 'draggenthar', 
                     'leap_year': 3}

    clock = GameClock(cal, starting_time)

Now the clock is formatted to use the custom calendar:

.. code:: Python

    >>> clock.prettytime
    '12:66 - Draggenthar, Winter 24, 33'

