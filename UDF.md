UDF is HTTP-based protocol designed to deliver data to Charting Library in a simple and efficient way. If you want to use UDF protocol you should create server-side HTTP service. 

#Response-as-a-table concept

Datafeeed responses often may be treated as tables. I.e., response about exchange’s symbols list may be treated as a table where each symbol represents a row, and there are some columns (minimal_price_movement, description, has_intraday e.t.c.). Each column may be an array (thus, it will provide separate value for each table’s row). But there may be a situation when all table’s rows has the same values of the column. In this case, the column’s value may be a single value in JSON response .

Example:

Let’s suppose we’ve requested the symbols list of exchange named NYSE. The response (in pseudo-format) might look like

```javascript
{
   symbols: ["MSFT", "AAPL", "FB", "GOOG"],
   min_price_move: 0.1,
   description: ["Microsoft corp.", "Apple Inc", "Facebook", "Google"]
}
```

If we try to imagine this response as a table, it will look like

Symbol|min_price_move|Description
---|---|---
MSFT|0.1|Microsoft corp.
AAPL|0.1|Apple Inc
FB|0.1|Facebook
GOOG|0.1|Google


#API Calls

###Datafeed configuration data
Request: `GET /config`

Response: Library expects to receive JSON of the same structure as for JS API [[setup() call|/JS-Api#setupreserved-callback]]. Also there should be 2 additional properties:

* **supports_search**: Set this one to `true` if your datafeed supports symbol search and individual symbol resolve logic.
* **supports_group_request**: Set this one to `true`  if your datafeed provides full info about symbols group only and is not able to perform symbol search or individual symbol resolve.

Either `supports_search` or `supports_group_request` should be `true`.

**Remark**: If your datafeed does not implement this call (do not respond at all or send 404), default configuration is used. Here it is:
```javascript
{
	supports_search: false,
	supports_group_request: true,
	supported_resolutions: ["1", "5", "15", "30", "60", "1D", "1W", "1M"],
	supports_marks: false
}
```

###Group symbols info
Request: `GET /symbol_info?group=<group_name>`
Example: `GET /symbol_info?group=NYSE`

Response: Response is expected to be an object with properties listed below. Each property is treated as table column, like described above (see [[response-as-a-table|UDF#response-as-a-table-concept]]). The response structure is similar (but not equal) to [[SymbolInfo|Symbology#symbolinfo-structure]] so see its description for details about all fields.

* **symbol**
* **description**
* **exchange-listed** / **exchange-traded**
* **minmov** / **minmovement**
* **minmov2**
* **pricescale**
* **has-intraday**
* **has-no-volume**
* **type**
* **ticker**
* **timezone**
* **session-regular** (mapped to `SymbolInfo.session`)
* **supported-resolutions**
* **force-session-rebuild**
* **has-daily**
* **intraday-multipliers**
* **has-fractional-volume** (obsolete)
* **volume-precision**
* **has-weekly-and-monthly**
* **has-empty-bars**

**Remark**: This call will be used if your datafeed sent `supports_group_request: true` in configuration data or had not responded to configuration request at all.

Example: Here is the example of datafeed response to `GET /symbol_info?group=NYSE` (all data is artificial):
```javascript
{
   symbol: ["AAPL", "MSFT", "SPX"],
   description: ["Apple Inc", "Microsoft corp", "S&P 500 index"],
   exchange-listed: "NYSE",
   exchange-traded: "NYSE",
   minmov: 1,
   minmov2: 0,
   pricescale: [1, 1, 100],
   has-dwm: true,
   has-intraday: true,
   has-no-volume: [false, false, true]
   type: ["stock, "stock", "index"],
   ticker: ["AAPL~0", "MSFT~0", "$SPX500"],
   timezone: “America/New_York”,
   session-regular: “0900-1600”,
}
```