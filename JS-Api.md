**What is JS API?**: A set of JS methods (specific public interface).

**Which steps should I follow to start using it?**: You should create a JS object that will receive data by some way and respond to Charting Library requests.

Data caching (history & symbol info) is implemented in the Charting Library.

When you create an object that implements the described interface simply pass it to Library widget constructor through [`datafeed` argument](Widget-Constructor#datafeed).

## Methods

1. [onReady](#onreadycallback)
1. [searchSymbols](#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback)
1. [resolveSymbol](#resolvesymbolsymbolname-onsymbolresolvedcallback-onresolveerrorcallback)
1. [getBars](#getbarssymbolinfo-resolution-from-to-onhistorycallback-onerrorcallback-firstdatarequest)
1. [subscribeBars](#subscribebarssymbolinfo-resolution-onrealtimecallback-subscriberuid-onresetcacheneededcallback)
1. [unsubscribeBars](#unsubscribebarssubscriberuid)
1. [calculateHistoryDepth](#calculatehistorydepthresolution-resolutionback-intervalback)
1. [getMarks](#getmarkssymbolinfo-from-to-ondatacallback-resolution)
1. [getTimescaleMarks](#gettimescalemarkssymbolinfo-from-to-ondatacallback-resolution)
1. [getServerTime](#getservertimecallback)

:chart: [Trading Terminal](Trading-Terminal) specific:

1. [getQuotes](#getquotessymbols-ondatacallback-onerrorcallback)
1. [subscribeQuotes](#subscribequotessymbols-fastsymbols-onrealtimecallback-listenerguid)
1. [unsubscribeQuotes](#unsubscribequoteslistenerguid)
1. [subscribeDepth](#subscribedepthsymbolinfo-callback-string)
1. [unsubscribeDepth](#unsubscribedepthsubscriberuid)

### onReady(callback)

1. `callback`: function(configurationData)
    1. `configurationData`: object (see below)

This call is intended to provide the object filled with the configuration data.
This data partially affects the chart behavior and is called [server-side customization](Customization-Overview#customization-done-through-data-stream).
Charting Library assumes that you will call the callback function and pass your datafeed `configurationData` as an argument.
Configuration data is an object; for now, the following properties are supported:

#### exchanges

An array of exchange descriptors. Exchange descriptor is an object `{value, name, desc}`. `value` will be passed as `exchange` argument to [searchSymbols](#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback).

`exchanges = []`  leads to the absence of the exchanges filter in Symbol Search list. Use `value = ""` if you wish to include all exchanges.

#### symbols_types

An array of filter descriptors. Filter descriptor is an object `{name, value}`. `value` will be passed as `symbolType` argument to [searchSymbols](#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback).

`symbolsTypes = []`  leads to the absence of filter types in Symbol Search list. Use `value = ""` if you wish to include all filter types.

#### supported_resolutions

An array of supported resolutions. Resolution must be a string. Format is described in another [article](Resolution).

`supported_resolutions = undefined` or `supported_resolutions = []` leads to resolution widget including the default content.

Example: `["1", "15", "240", "D", "6M"]` will give you "1 minute, 15 minutes, 4 hours, 1 day, 6 months" in resolution widget.

#### supports_marks

Boolean showing whether your datafeed supports marks on bars or not.

#### supports_timescale_marks

Boolean showing whether your datafeed supports timescale marks or not.

#### supports_time

Set this one to `true` if your datafeed provides server time (unix time). It is used to adjust Countdown on the Price scale.

#### futures_regex

Set it if you want to group futures in the symbol search. This REGEX should divide an instrument name into 2 parts: a root and an expiration.

Sample regex: : `/^(.+)([12]!|[FGHJKMNQUVXZ]\d{1,2})$/`. It will be applied to the instruments with `futures` as a `type`.

### searchSymbols(userInput, exchange, symbolType, onResultReadyCallback)

1. `userInput`: string. It is text entered by user in the symbol search field.
1. `exchange`: string. The requested exchange (chosen by user). Empty value means no filter was specified.
1. `symbolType`: string. The requested symbol type: `index`, `stock`, `forex`, etc (chosen by user).
    Empty value means no filter was specified.
1. `onResultReadyCallback`: function(result)
    1. `result`: array (see below)

This call is intended to provide the list of symbols that match the user's search query. `result` is expected to look like the following:

```javascript
[
    {
        "symbol": "<short symbol name>",
        "full_name": "<full symbol name>", // e.g. BTCE:BTCUSD
        "description": "<symbol description>",
        "exchange": "<symbol exchange name>",
        "ticker": "<symbol ticker name, optional>",
        "type": "stock" // or "futures" or "bitcoin" or "forex" or "index"
    },
    {
        //    .....
    }
]
```

If no symbols are found, then callback should be called with an empty array. See more details about `ticker` value [here](Symbology#ticker)

### resolveSymbol(symbolName, onSymbolResolvedCallback, onResolveErrorCallback)

1. `symbolName`: string. Symbol name or `ticker` if provided.
1. `onSymbolResolvedCallback`: function([SymbolInfo](Symbology#symbolinfo-structure))
1. `onResolveErrorCallback`: function(reason)

Charting Library will call this function when it needs to get [SymbolInfo](Symbology#symbolinfo-structure) by symbol name.

### getBars(symbolInfo, resolution, from, to, onHistoryCallback, onErrorCallback, firstDataRequest)

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `resolution`: string
1. `from`: unix timestamp, leftmost required bar time
1. `to`: unix timestamp, rightmost required bar time
1. `onHistoryCallback`: function(array of `bar`s, `meta` = `{ noData = false }`)
    1. `bar`: object `{time, close, open, high, low, volume}`
    1. `meta`: object `{noData = true | false, nextTime - unix time}`
1. `onErrorCallback`: function(reason)
1. `firstDataRequest`: boolean to identify the first call of this method for the particular symbol resolution.
    When it is set to `true` you can ignore `to` (which depends on browser's `Date.now()`) and return bars up to the latest bar.

This function is called when the chart needs a history fragment defined by dates range.

The charting library assumes `onHistoryCallback` to be called **just once**.

**Important**: `nextTime` is a time of the next bar in the history. It should be set only in the event that there is no data in the requested period.

**Important**: `noData` should be set only in the event that there is no data in the requested and earlier period.

**Remark**: `bar.time` is expected to be the amount of milliseconds since Unix epoch start in **UTC** timezone.

**Remark**: `bar.time` for daily bars is expected to be a trading day (not session start day) at 00:00 UTC.
Charting Library adjusts time according to [Session](Symbology#session) from SymbolInfo

**Remark**: `bar.time` for monthly bars is the first trading day of the month without the time part

### subscribeBars(symbolInfo, resolution, onRealtimeCallback, subscriberUID, onResetCacheNeededCallback)

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `resolution`: string
1. `onRealtimeCallback`: function(bar)
    1. `bar`: object `{time, close, open, high, low, volume}`
1. `subscriberUID`: object
1. `onResetCacheNeededCallback` *(since version 1.7)*: function() to be executed when bar data has changed

Charting Library calls this function when it wants to receive real-time updates for a symbol. The Library assumes that you will call `onRealtimeCallback` every time you want to update the most recent bar or to add a new one.

**Remark**: When you call `onRealtimeCallback` with bar having time equal to the most recent bar's time then the entire last bar is replaced with the `bar` object you've passed into the call.

Example:

1. The most recent bar is `{1419411578413, 10, 12, 9, 11}`
1. You call `onRealtimeCallback({1419411578413, 10, 14, 9, 14})`
1. Library finds out that the bar with the time `1419411578413` already exists and is the most recent one
1. Library replaces the entire bar making the most recent bar `{1419411578413, 10, 14, 9, 14}`

**Remark 2**: Is it possible either to update the most recent bar or to add a new one with `onRealtimeCallback`.
You'll get an error if you call this function when trying to update a historical bar.

**Remark 3**: There is no way to change historical bars once they've been received by the chart currently.

### unsubscribeBars(subscriberUID)

1. `subscriberUID`: object

Charting Library calls this function when it doesn't want to receive updates for this subscriber any more. `subscriberUID` will be the same object that Library passed to `subscribeBars` before.

### calculateHistoryDepth(resolution, resolutionBack, intervalBack)

*Optional.*

1. `resolution`: requested symbol resolution
1. `resolutionBack`: time period types. Supported values are: `D` | `M`
1. `intervalBack`: amount of `resolutionBack` periods that the Charting Library is going to request

Charting Library calls this function when it is going to request some historical data to give you an ability to override the amount of bars requested.

It passes some arguments so that you are aware of the amount of bars it's going to get. Here are some examples:

* `calculateHistoryDepth("D", "M", 12)` called: the Library is going to request 12 months of daily bars
* `calculateHistoryDepth("60", "D", 15)` called: the Library is going to request 15 days of hourly bars

This function should return `undefined` if you do not wish to override anything.
If you do, it should return an object `{resolutionBack, intervalBack}`.

Example:

Let's assume that the implementation is as follows

```javascript
Datafeed.prototype.calculateHistoryDepth = function(resolution, resolutionBack, intervalBack) {
    if (resolution === "1D") {
        return {
            resolutionBack: 'M',
            intervalBack: 6
        };
    }
}
```

When the Charting Library requests the data for `1D` resolution, the history will be 6 months deep.
In all other cases the history depth will have the default value.

### getMarks(symbolInfo, from, to, onDataCallback, resolution)

*Optional.*

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `from`: unix timestamp (UTC). Leftmost visible bar's time.
1. `to`: unix timestamp (UTC). Rightmost visible bar's time.
1. `onDataCallback`: function(array of `mark`s)
1. `resolution`: string

The Library calls this function to get [marks](Marks-On-Bars) for visible bars range.

The Library assumes that you will call `onDataCallback` only once per `getMarks` call.

`mark` is an object that has the following properties:

* `id`: unique mark ID. It will be passed to a [respective callback](Widget-Methods#subscribeevent-callback) when user clicks on a mark
* `time`: unix time, UTC
* `color`: `red` | `green` | `blue` | `yellow` | `{ border: '#ff0000', background: '#00ff00' }`
* `text`: mark popup text. HTML supported
* `label`: a letter to be printed on a mark. Single character
* `labelFontColor`: color of a letter on a mark
* `minSize`: minimum mark size (diameter, pixels) (default value is `5`)

A few marks per bar are allowed (for now, the maximum is `10`). Marks outside of the bars are not allowed.

**Remark**: This function will be called only if you confirmed that your back-end is [supporting marks](#supports_marks).

### getTimescaleMarks(symbolInfo, from, to, onDataCallback, resolution)

*Optional.*

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `from`: unix timestamp (UTC). Leftmost visible bar's time.
1. `to`: unix timestamp (UTC). Rightmost visible bar's time.
1. `onDataCallback`: function(array of `mark`s)
1. `resolution`: string

The Library calls this function to get timescale marks for visible bars range.

The Library assumes that you will call `onDataCallback` only once per `getTimescaleMarks` call.

`mark` is an object that has the following properties:

* `id`: unique mark ID. Will be passed to a [respective callback](Widget-Methods#subscribeevent-callback) when user clicks on a mark
* `time`: unix time, UTC
* `color`: `red` | `green` | `blue` | `yellow` | ... | `#000000`
* `label`: a letter to be printed on a mark. Single character
* `tooltip`: array of text strings. Each element of the array is a new text line of a tooltip.

Only one mark per bar is allowed. Marks outside of the bars are not allowed.

**Remark**: This function will be called only if you confirmed that your back-end is [supporting marks](#supports_timescale_marks).

### getServerTime(callback)

1. `callback`: function(unixTime)

This function is called if the configuration flag `supports_time` is set to `true` when the Charting Library needs to know the server time.

The Charting Library assumes that the callback is called once.

The time is provided without milliseconds.

It is used to display Countdown on the price scale.

Example: `1445324591`.

## [Trading Terminal](Trading-Terminal) specific

### getQuotes(symbols, onDataCallback, onErrorCallback)

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `symbols`: array of symbols names
1. `onDataCallback`: function(array of `data`)
    1. `data`: [symbol quote data](Quotes#symbol-quote-data)
1. `onErrorCallback`: function(reason)

This function is called when the Charting Library needs quote data. The charting library assumes that `onDataCallback` is called once when all the requested data is received.

### subscribeQuotes(symbols, fastSymbols, onRealtimeCallback, listenerGUID)

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `symbols`: array of symbols that should be updated rarely (once per minute). These symbols are included in the watchlist but they are not visible at the moment.
1. `fastSymbols`: array of symbols that should be updated frequently (once every 10 seconds or more often)
1. `onRealtimeCallback`: function(array of `data`)
    1. `data`: [symbol quote data](Quotes#symbol-quote-data)
1. `listenerGUID`: unique identifier of the listener

Trading Terminal calls this function when it wants to receive real-time quotes for a symbol.

The Charting Library assumes that you will call `onRealtimeCallback` every time you want to update the quotes.

### unsubscribeQuotes(listenerGUID)

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `listenerGUID`: unique identifier of the listener

Trading Terminal calls this function when it doesn't want to receive updates for this listener anymore.

`listenerGUID` will be the same object that the Library passed to `subscribeQuotes` before.

### subscribeDepth(symbolInfo, callback): string

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `callback`: function(depth)
    1. `depth`: object `{snapshot, asks, bids}`
        * `snapshot`: Boolean - if `true` `asks` and `bids` have full set of depth, otherwise they contain only updated levels.
        * `asks`: Array of `{price, volume}`
        * `bids`: Array of `{price, volume}`

Trading Terminal calls this function when it wants to receive real-time level 2 (DOM) for a symbol. The Charting Library assumes that you will call the `callback` every time you want to update DOM data.

This method should return a unique identifier (`subscriberUID`) that will be used to unsubscribe from the data.

### unsubscribeDepth(subscriberUID)

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `subscriberUID`: String

Trading Terminal calls this function when it doesn't want to receive updates for this listener anymore.

`subscriberUID` will be the same object that was returned from `subscribeDepth`.
