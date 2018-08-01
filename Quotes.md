Quote is a short set of data that describes the current price. Charting Library supports watchlists (in **Trading Terminal** configuration) and uses quotes to display symbol information.

Charting Library uses the same data structures for quotes in both [JS API](JS Api) and [UDF](UDF). Here is the description of response object:

# Symbol Quote Data

* `s`: Status code for symbol. Expected values: `ok` | `error`
* `n`: Symbol name. This value must be **exactly the same** as in the request
* `v`: object, symbol quote itself
  * `ch`: price change (usually counts as an open price on a particular day)
  * `chp`: price change percentage
  * `short_name`: short name of the symbol
  * `exchange`: the exchange name
  * `description`: short description of the symbol
  * `lp`: last traded price
  * `ask`: ask price
  * `bid`: bid price
  * `spread`: spread
  * `open_price`: today's open
  * `high_price`: today's high
  * `low_price`: today's low
  * `prev_close_price`: yesterday's close
  * `volume`: today's volume
