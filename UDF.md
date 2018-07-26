**What is UDF?** It's an HTTP-based protocol that is designed to deliver data to the Charting Library in a simple and efficient way.

**How can I start using it?** You should create a tiny server-side HTTP service that will get the data from your storage and respond to Charting Library requests.

## Response-as-a-table concept

Think of data feed responses as tables. For example, a data feed response that includes a symbol list from the exchange may be treated as a table where each symbol represents a row, along with some columns (minimal_price_movement, description, has_intraday e.t.c.).
Each column may be an array (it will provide a separate value for each row in a table).
Note that there might be a situation when all rows have the same value in the same column.
In this case, the column value can be defined as a single value in JSON response.

Example:

Let's assume that we requested a symbol list from the New York Stock Exchange. The response (in pseudo-format) might look like

```javascript
{
   symbols: ["MSFT", "AAPL", "FB", "GOOG"],
   min_price_move: 0.1,
   description: ["Microsoft corp.", "Apple Inc", "Facebook", "Google"]
}
```

Here is how this response will look like in a table format.

Symbol|min_price_move|Description
---|---|---
MSFT|0.1|Microsoft corp.
AAPL|0.1|Apple Inc
FB|0.1|Facebook
GOOG|0.1|Google

## API Calls

### Data feed configuration data

Request: `GET /config`

Response: Library expects to receive a JSON response of the same structure as a result of JS API [setup() call](JS-Api#onreadycallback).

Also there should be 2 additional properties:

* `supports_search`: Set it to `true` if your data feed supports symbol search and individual symbol resolve logic.
* `supports_group_request`: Set it to `true`  if your data feed provides full information on symbol group only and is not able to perform symbol search or individual symbol resolve.

Either `supports_search` or `supports_group_request` should be set to `true`.

**Remark**: If your data feed doesn't implement this call (doesn't respond or sends 404 error) then the default configuration is being used. Here is the default configuration:

```javascript
{
    supported_resolutions: ['1', '5', '15', '30', '60', '1D', '1W', '1M'],
    supports_group_request: true,
    supports_marks: false,
    supports_search: false,
    supports_timescale_marks: false,
}
```

### Symbol group request

Request: `GET /symbol_info?group=<group_name>`

1. `group_name`: string

Example: `GET /symbol_info?group=NYSE`

Response: Response is expected to be an object with properties listed below.
Each property is treated as table column, as described above (see [response-as-a-table](UDF#response-as-a-table-concept)).
The response structure is similar (but **not equal**) to [SymbolInfo](Symbology#symbolinfo-structure) so please read the description to learn about the details of each field.

* `symbol`
* `description`
* `exchange-listed` / `exchange-traded`
* `minmovement` / `minmov` (NOTE: `minmov` is deprecated and will be removed in future releases)
* `minmovement2` / `minmov2` (NOTE: `minmov2` is deprecated and will be removed in future releases)
* `fractional`
* `pricescale`
* `has-intraday`
* `has-no-volume`
* `type`
* `ticker`
* `timezone`
* `session-regular` (mapped to `SymbolInfo.session`)
* `supported-resolutions`
* `force-session-rebuild`
* `has-daily`
* `intraday-multipliers`
* `volume_precision`
* `has-weekly-and-monthly`
* `has-empty-bars`

Here is an example of data feed response to `GET /symbol_info?group=NYSE` request:

```javascript
{
   symbol: ["AAPL", "MSFT", "SPX"],
   description: ["Apple Inc", "Microsoft corp", "S&P 500 index"],
   exchange-listed: "NYSE",
   exchange-traded: "NYSE",
   minmovement: 1,
   minmovement2: 0,
   pricescale: [1, 1, 100],
   has-dwm: true,
   has-intraday: true,
   has-no-volume: [false, false, true]
   type: ["stock", "stock", "index"],
   ticker: ["AAPL~0", "MSFT~0", "$SPX500"],
   timezone: “America/New_York”,
   session-regular: “0900-1600”,
}
```

**Remark 1**: This call will be used if your data feed sent `supports_group_request: true` in the configuration data or didn't respond to the configuration request at all.

**Remark 2**: In the event that your data feed does not support the requested symbol group (which should not happen if your response to request #1 (supported groups) is correct) you may expect a 404 error.

**Remark 3**: When using this mode of receiving data (getting large amount of symbol data) your browser will keep the data that wasn't even requested by the user.
If your symbol list has more than a few items, please consider supporting symbol search / individual symbol resolve instead.

### Symbol resolve

Request: `GET /symbols?symbol=<symbol>`

1. `symbol`: string. Symbol name or ticker.

Example: `GET /symbols?symbol=AAL`, `GET /symbols?symbol=NYSE:MSFT`

A JSON response of the same structure as [SymbolInfo](Symbology#symbolinfo-structure)

**Remark**: This call will be requested if your data feed sent `supports_group_request: false` and `supports_search: true` in the configuration data.

### Symbol search

Request: `GET /search?query=<query>&type=<type>&exchange=<exchange>&limit=<limit>`

* `query`: string. Text typed by the user in the Symbol Search edit box
* `type`: string. One of the symbol types [supported](JS-Api#symbols_types) by your back-end
* `exchange`: string. One of the exchanges [supported](JS-Api#exchanges) by your back-end
* `limit`: integer. The maximum number of symbols in a response

Example: `GET /search?query=AA&type=stock&exchange=NYSE&limit=15`

A response is expected to be an array of symbol objects as in [respective JS API call](JS-Api#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback)

**Remark**: This call will be requested if your data feed sent `supports_group_request: false` and `supports_search: true` in the configuration data.

### Bars

Request: `GET /history?symbol=<ticker_name>&from=<unix_timestamp>&to=<unix_timestamp>&resolution=<resolution>`

* `symbol`: symbol name or ticker.
* `from`: unix timestamp (UTC) of leftmost required bar
* `to`: unix timestamp (UTC) of rightmost required bar
* `resolution`: string

Example: `GET /history?symbol=BEAM~0&resolution=D&from=1386493512&to=1395133512`

A response is expected to be an object with some properties listed below. Each property is treated as a table column, as described above.

* `s`: status code. Expected values: `ok` | `error` | `no_data`
* `errmsg`: Error message. Should be present only when `s = 'error'`
* `t`: Bar time. Unix timestamp (UTC)
* `c`: Closing price
* `o`: Opening price (optional)
* `h`: High price (optional)
* `l`: Low price (optional)
* `v`: Volume (optional)
* `nextTime`: Time of the next bar if there is no data (status code is `no_data`) in the requested period (optional)

**Remark**: Bar time for daily bars should be 00:00 UTC and is expected to be a trading day (not a day when the session starts).
Charting Library aligns the time according to the [Session](Symbology#session) from SymbolInfo.

**Remark**: Bar time for monthly bars should be 00:00 UTC and is the first trading day of the month.

**Remark**: Prices should be passed as numbers and not as strings in quotation marks.

Example:

```javascript
{
   s: "ok",
   t: [1386493512, 1386493572, 1386493632, 1386493692],
   c: [42.1, 43.4, 44.3, 42.8]
}
```

```javascript
{
   s: "no_data",
   nextTime: 1386493512
}
```

```javascript
{
   s: "ok",
   t: [1386493512, 1386493572, 1386493632, 1386493692],
   c: [42.1, 43.4, 44.3, 42.8],
   o: [41.0, 42.9, 43.7, 44.5],
   h: [43.0, 44.1, 44.8, 44.5],
   l: [40.4, 42.1, 42.8, 42.3],
   v: [12000, 18500, 24000, 45000]
}
```

#### How `nextTime` works

Let's assume that a user opened the chart where `resolution = 1` and the Library requests the following range of data from the data feed `[3 Apr 2015 16:00 UTC+0, 3 Apr 2015 19:00 UTC+0]` for a stock that is traded on the NYSE.
April 3rd was Good Friday which means that the markets were closed.
Library expects the following response from the data feed.

```javascript
{
  s: "no_data",
  nextTime: 1428001140000 // 2 Apr 2015 18:59:00 GMT+0
}
```

`nextTime` is the time of the closest available bar in the past.

### Marks

Request: `GET /marks?symbol=<ticker_name>&from=<unix_timestamp>&to=<unix_timestamp>&resolution=<resolution>`

* `symbol`: symbol name or ticker.
* `from`: unix timestamp (UTC) of leftmost visible bar
* `to`: unix timestamp (UTC) of rightmost visible bar
* `resolution`: string

A response is expected to be an object with some properties listed below.
This object is similar to [respective response](JS-Api#getmarkssymbolinfo-from-to-ondatacallback-resolution) in JS API, but each property is treated as a table column, as described above.

```javascript
{
    id: [array of ids],
    time: [array of times],
    color: [array of colors],
    text: [array of texts],
    label: [array of labels],
    labelFontColor: [array of label font colors],
    minSize: [array of minSizes],
}
```

**Remark**: This call will be requested if your data feed sent `supports_marks: true` in the configuration data.

### Timescale marks

Request: `GET /timescale_marks?symbol=<ticker_name>&from=<unix_timestamp>&to=<unix_timestamp>&resolution=<resolution>`

* `symbol`: symbol name or ticker.
* `from`: unix timestamp (UTC) or leftmost visible bar
* `to`: unix timestamp (UTC) or rightmost visible bar
* `resolution`: string

A response is expected to be an array of objects with properties listed below.

* `id`: unique identifier of a mark
* `color`: rgba color
* `label`: a letter to be displayed in a circle
* `time`: unix time
* `tooltip`: tooltip text

**Remark**: This call will be requested if your data feed sent `supports_timescale_marks: true` in the configuration data.

### Server time

Request: `GET /time`

Response: Numeric unix time without milliseconds.

Example: `1445324591`

### Quotes

Request: `GET /quotes?symbols=<ticker_name_1>,<ticker_name_2>,...,<ticker_name_n>`

Example: `GET /quotes?symbols=NYSE%3AAA%2CNYSE%3AF%2CNasdaqNM%3AAAPL`

A response is an object with the following keys.

* `s`: Status code for the request. Expected values are: `ok` or `error`
* `errmsg`: Error message. Should be present only when `s = 'error'`
* `d`: [symbols data](Quotes) Array

Example:

```json
{
    "s": "ok",
    "d": [
        {
            "s": "ok",
            "n": "NYSE:AA",
            "v": {
                "ch": "+0.16",
                "chp": "0.98",
                "short_name": "AA",
                "exchange": "NYSE",
                "description": "Alcoa Inc. Common",
                "lp": "16.57",
                "ask": "16.58",
                "bid": "16.57",
                "open_price": "16.25",
                "high_price": "16.60",
                "low_price": "16.25",
                "prev_close_price": "16.41",
                "volume": "4029041"
            }
        },
        {
            "s": "ok",
            "n": "NYSE:F",
            "v": {
                "ch": "+0.15",
                "chp": "0.89",
                "short_name": "F",
                "exchange": "NYSE",
                "description": "Ford Motor Company",
                "lp": "17.02",
                "ask": "17.03",
                "bid": "17.02",
                "open_price": "16.74",
                "high_price": "17.08",
                "low_price": "16.74",
                "prev_close_price": "16.87",
                "volume": "7713782"
            }
        }
    ]
}
```

## Constructor

`Datafeeds.UDFCompatibleDatafeed = function(datafeedURL, updateFrequency)`

### datafeedURL

This is a URL of a data server that will receive requests and return data.

### updateFrequency

This is a frequency of requests that the data feed will send to the server in milliseconds. Default is 10000 (10 sec).
