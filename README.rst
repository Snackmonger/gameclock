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
video game. The clock is controlled by calling the ``GameClock.tick()`` method,
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