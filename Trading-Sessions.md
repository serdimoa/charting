Trading session of a particular symbol is passed to the Charting Library in the `session` field of the symbol information.
Session time format is `HHMM-HHMM` in the Charting Library.
E.g., A session that starts at 9:30am and ends at 4:00 pm should look like `0930-1600`.
There is a special case for symbols traded 24/7 (e.g., Bitcoin and other cryptocurrencies): the session string should be `24x7`.

Session time is expected to be in **exchange time zone**.

If session start time is greater than end time (ie, `1700-0900`), this session will be treated as an overnight one.

Overnight session always starts on the previous day: e.g., if the symbol is traded Monday-Friday `1700-0900`, then the first session of the week will be treated as if it started at 17:00 of the previous week and ended at 09:00 on Monday of this week.

There may be more than one session in a trading day. If that's the case, you should pass all of them as sessions separated with comma.
E.g., assuming you have 2 trading sessions within a single day - 9:30 to 14:00 and then from 14:30 to 17:00, the day session string should be `0930-1400,1430-1700`.

Also, there may be a situation when trading hours differ from day to day.
You can use the `:` specifier to assign the day the session should be applied to.
E.g., if the symbol is traded between 0900-1630 every day except for Monday (on Mondays, the trading session starts at 0900 and ends at 1400),then the session string should be `0900-1630|0900-1400:2`.

Let's see this string in details.

Fragment | Meaning
---------|--------
0900-1630|A session 0900-1630. This session will be assigned by default to all non-weekend days because it's not followed by the `:` specifier.
&#124;|Sessions separator. This character divides different sessions.
0900-1400|A session 0900-1400. It's the session for a day #2 (see below)
:|Day specifier. This character follows the session hours and is followed by the day number.
2|The day number for the session above (0900-1400)

**Day numbers** are `1` for **Sunday** and `7` for **Saturday** (`2` - Monday, `3` - Tuesday, etc.).

You can specify more than 1 day for a single session.
E.g., in `0900-1630|0900-1400:23` `0900-1400` sessions will be assigned to days `2` and `3` (Monday, Tuesday).

**version: 1.1**:

You can specify the first trading day of the week using semicolon. Examples:

- `1;0900-1630|0900-1400:2` - first day of the week is Sunday
- `0900-1630|0900-1400:2;6` - first day of the week is Saturday
- `0900-1630|0900-1400:2` - first day of the week is Monday (default value)

**Use this parser to check your session string**: <http://tradingview.github.io/checksession.html>
