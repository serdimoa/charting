**What it is?**: A set of JS methods (specific public interface).

**What should I do to use it?**: You should create a JS object which will receive the data by some way and respond to Charting Library requests.

Data caching (history & symbol info) is implemented in Charting Library.

When you create an object implementing described interface, just pass it to Library widget constructor through [`datafeed` argument](Widget-Constructor.md#datafeed).

## Methods

1. [onReady](#onreadycallback)
1. [searchSymbols](#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback)
1. [resolveSymbol](#resolvesymbolsymbolname-onsymbolresolvedcallback-onresolveerrorcallback)
1. [getBars](#getbarssymbolinfo-resolution-from-to-onhistorycallback-onerrorcallback-firstdatarequest)
1. [subscribeBars](#subscribebarssymbolinfo-resolution-onrealtimecallback-subscriberuid-onresetcacheneededcallback)
1. [unsubscribeBars](#unsubscribebarssubscriberuid)
1. [calculateHistoryDepth](#calculatehistorydepthresolution-resolutionback-intervalback)
1. [getMarks](#getmarkssymbolinfo-startdate-enddate-ondatacallback-resolution)
1. [getTimescaleMarks](#gettimescalemarkssymbolinfo-startdate-enddate-ondatacallback-resolution)
1. [getServerTime](#getservertimecallback)

:chart: [Trading Terminal](Trading-Terminal.md) specific:

1. [getQuotes](#getquotessymbols-ondatacallback-onerrorcallback)
1. [subscribeQuotes](#subscribequotessymbols-fastsymbols-onrealtimecallback-listenerguid)
1. [unsubscribeQuotes](#unsubscribequoteslistenerguid)
1. [subscribeDepth](#subscribedepthsymbolinfo-callback-string)
1. [unsubscribeDepth](#unsubscribedepthsubscriberuid)

### onReady(callback)

1. `callback`: function(configurationData)
    1. `configurationData`: object (see below)

This call is intended to provide the object filled with configuration data.
This data affects some of chart behavior aspects so it is called [server-side customization](Customization-Overview.md#customization-done-through-data-stream).
Charting Library expects you will call callback and pass your datafeed `configurationData` as an argument.
Configuration data is an object; for now, following properties are supported:

#### exchanges

An array of exchange descriptors. Exchange descriptor is an object `{value, name, desc}`. `value` will be passed as `exchange` argument to [searchSymbols](#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback).

`exchanges = []`  leads to exchanges filter absence in Symbol Search list. Use `value = ""` if you want to create wildcard filter (all exchanges).

#### symbols_types

An array of filter descriptors. Filter descriptor is an object `{name, value}`. `value` will be passed as `symbolType` argument to [searchSymbols](#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback).

`symbolsTypes = []`  leads to types filter absence in Symbol Search list. Use `value = ""` if you want to create wildcard filter (all types).

#### supported_resolutions

An array of supported resolutions. Resolution must be a string. Format is described in another [article](Resolution.md).

`supported_resolutions = undefined` or `supported_resolutions = []` leads to resolutions widget having its default content.

Example: `["1", "15", "240", "D", "6M"]` will give you "1 minute, 15 minutes, 4 hours, 1 day, 6 months" in resolution widget.

#### supports_marks

Boolean showing whether your datafeed supports marks on bars or not.

#### supports_timescale_marks

Boolean showing whether your datafeed supports timescale marks or not.

#### supports_time

Set this one to `true` if your datafeed provides server time (unix time). It is used to adjust Countdown on the Price scale.

#### futures_regex

Set it if you want to group futures in the symbol search. This REGEX should divide an instrument name in 2 parts: a root and an expiration.

Sample regex: : `/^(.+)([12]!|[FGHJKMNQUVXZ]\d{1,2})$/`. It will be applied to the instruments whose `type` is `futures`.

### searchSymbols(userInput, exchange, symbolType, onResultReadyCallback)

1. `userInput`: string. It is text entered by user in symbol search field
1. `exchange`: string. The requested exchange (chosen by user). Empty value means no filter was specified.
1. `symbolType`: string. The requested symbol type: `index`, `stock`, `forex`, etc (chosen by user).
    Empty value means no filter was specified.
1. `onResultReadyCallback`: function(result)
    1. `result`: array (see below)

This call is intended to provide the list of symbols matching to user's search query. `result` is expected to be something like this:

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

If no symbols are found, then callback should be called with an empty array. See more details about `ticker` value [here](Symbology.md#ticker)

### resolveSymbol(symbolName, onSymbolResolvedCallback, onResolveErrorCallback)

1. `symbolName`: string. Symbol name or `ticker` if provided.
1. `onSymbolResolvedCallback`: function([SymbolInfo](Symbology.md#symbolinfo-structure))
1. `onResolveErrorCallback`: function(reason)

Charting Library will call this function when it need to get [SymbolInfo](Symbology.md#symbolinfo-structure) by symbol's name.

### getBars(symbolInfo, resolution, from, to, onHistoryCallback, onErrorCallback, firstDataRequest)

1. `symbolInfo`: [SymbolInfo](Symbology.md#symbolinfo-structure) object
1. `resolution`: string
1. `from`: unix timestamp, leftmost required bar time
1. `to`: unix timestamp, rightmost required bar time
1. `onHistoryCallback`: function(array of `bar`s, `meta` = `{ noData = false }`)
    1. `bar`: object `{time, close, open, high, low, volume}`
    1. `meta`: object `{noData = true | false, nextTime - unix time}`
1. `onErrorCallback`: function(reason)
1. `firstDataRequest`: boolean to identify the first history call for this symbol/resolution.
    When it is `true` you can ignore `to` (which depends on browser's `Date.now()`) and return bars up to current bar (including it).

This function is called when chart needs a history fragment defined by dates range.

The charting library expects `onHistoryCallback` to be called **just once** after receiving all the requesting history.
No further calls are expected.

**Important**: `nextTime` is a time of the next bar in the history. It should be set when there is no data in the requested period only.

**Important**: `noData` should be set when there is no data in the requested period and earlier only.

**Remark**: `bar.time` is expected to be the amount of milliseconds since Unix epoch start in **UTC** timezone.

**Remark**: `bar.time` for daily bars is expected to be a trading day (not session start day) at 00:00 UTC.
Charting Library aligns time according to [Session](Symbology.md#session) from SymbolInfo

**Remark**: `bar.time` for monthly bars is the first trading day of the month without the time part

### subscribeBars(symbolInfo, resolution, onRealtimeCallback, subscriberUID, onResetCacheNeededCallback)

1. `symbolInfo`: [SymbolInfo](Symbology.md#symbolinfo-structure) object
1. `resolution`: string
1. `onRealtimeCallback`: function(bar)
    1. `bar`: object `{time, close, open, high, low, volume}`
1. `subscriberUID`: object
1. `onResetCacheNeededCallback` *(since version 1.7)*: function() to be executed when bars data has changed

Charting Library calls this function when it wants to receive realtime updates for a symbol. Chart expects you will call `onRealtimeCallback` every time you want to update the most recent bar or to append a new one.

**Remark**: When you call `onRealtimeCallback` with bar having time equal to most recent bar's time, the whole last bar is replaced with the `bar` object you've passed into the call.

Example:

1. The most recent bar is `{1419411578413, 10, 12, 9, 11}`
1. You call `onRealtimeCallback({1419411578413, 10, 14, 9, 14})`
1. Library finds out that bar with time `1419411578413` already exists and is the most recent one
1. Library replaces the whole bar so now the most recent bar is `{1419411578413, 10, 14, 9, 14}`

**Remark 2**: Is it possible either to update the most recent bar or to append a new one with `onRealtimeCallback`.
You've get an error if you call this function trying to update a bar in history.

**Remark 3**: For now, there is no way to change bars in history after the chart received it.

### unsubscribeBars(subscriberUID)

1. `subscriberUID`: object

Library calls this function when is doesn't want to receive updates for this subscriber any more. `subscriberUID` will be the same object which Library passed to `subscribeBars` before.

### calculateHistoryDepth(resolution, resolutionBack, intervalBack)

*Optional.*

1. `resolution`: requested symbol's resolution
1. `resolutionBack`: desired history period dimension. Supported values: `D` | `M`
1. `intervalBack`: amount or `resolutionBack` periods which Library is going to request

Charting Library calls this function when it is going to request some history data to give you an ability to override required history depth.

It passes some arguments so you could know how much bars is it going to get. Here are a few examples:

* `calculateHistoryDepth("D", "M", 12)` called: the Library is going to request 12 months of daily bars
* `calculateHistoryDepth("60", "D", 15)` called: the Library is going to request 15 days of hourly bars

This function should return `undefined` if you do not want to override anything.
If you do, it should return an object `{resolutionBack, intervalBack}`.
Properties meaning is similar to respective arguments' one.

Example:

Assume the implementation is

```javascript
Datafeed.prototype.calculateHistoryDepth = function(resolution, resolutionBack, intervalBack) {
    if (period === "1D") {
        return {
            resolutionBack: 'M',
            intervalBack: 6
        };
    }
}
```

This means when Charting Library will request the data for `1D` resolution, the history will be 6 months in depth.
In all other cases the history depth will have the default value.

### getMarks(symbolInfo, startDate, endDate, onDataCallback, resolution)

*Optional.*

1. `symbolInfo`: [SymbolInfo](Symbology.md#symbolinfo-structure) object
1. `startDate`: unix timestamp (UTC). Leftmost visible bar's time.
1. `endDate`: unix timestamp (UTC). Rightmost visible bar's time.
1. `onDataCallback`: function(array of `mark`s)
1. `resolution`: string

Library calls this function to get [marks](Marks-On-Bars.md) for visible bars range.

Chart expects you to call `onDataCallback` only once per each `getMarks` call.

`mark` is an object having following properties:

* `id`: unique mark id. Will be passed to a [respective callback](Widget-Methods.md#subscribeevent-callback) when user clicks on a mark
* `time`: unix time, UTC
* `color`: `red` | `green` | `blue` | `yellow` | `{ border: '#ff0000', background: '#00ff00' }`
* `text`: mark popup text. HTML supported
* `label`: a letter to be printed on a mark. Single character
* `labelFontColor`: color of a letter on a mark
* `minSize`: minimal size of mark (diameter, pixels)

A few marks per bar are allowed (for now, maximum is `10`). Marks out of bars are not allowed.

**Remark**: This function will be called only if you declared your back-end is [supporting marks](#supports_marks).

### getTimescaleMarks(symbolInfo, startDate, endDate, onDataCallback, resolution)

*Optional.*

1. `symbolInfo`: [SymbolInfo](Symbology.md#symbolinfo-structure) object
1. `startDate`: unix timestamp (UTC). Leftmost visible bar's time.
1. `endDate`: unix timestamp (UTC). Rightmost visible bar's time.
1. `onDataCallback`: function(array of `mark`s)
1. `resolution`: string

Library calls this function to get timescale marks for visible bars range.

Chart expects you to call `onDataCallback` only once per each `getTimescaleMarks` call.

`mark` is an object having following properties:

* `id`: unique mark id. Will be passed to a [respective callback](Widget-Methods.md#subscribeevent-callback) when user clicks on a mark
* `time`: unix time, UTC
* `color`: `red` | `green` | `blue` | `yellow` | ... | `#000000`
* `label`: a letter to be printed on a mark. Single character
* `tooltip`: array of text strings. Each element of the array is a new text line of a tooltip.

Only one mark per bar is allowed. Marks out of bars are not allowed.

**Remark**: This function will be called only if you declared your back-end is [supporting marks](#supports_timescale_marks).

### getServerTime(callback)

1. `callback`: function(unixTime)

This function is called if configuration flag `supports_time` is set to `true` when chart needs to know the server time.

The charting library expects callback to be called once.

The time is provided without milliseconds.

It is used to display Countdown on the price scale.

Example: `1445324591`.

## [Trading Terminal](Trading-Terminal.md) specific

### getQuotes(symbols, onDataCallback, onErrorCallback)

:chart: *[Trading Terminal](Trading-Terminal.md) specific.*

1. `symbols`: array of symbols names
1. `onDataCallback`: function(array of `data`)
    1. `data`: [symbol quote data](Quotes.md#symbol-quote-data)
1. `onErrorCallback`: function(reason)

This function is called when chart needs quotes data. The charting library expects onDataCallback to be called once when all requesting data received. No further calls are expected.

### subscribeQuotes(symbols, fastSymbols, onRealtimeCallback, listenerGUID)

:chart: *[Trading Terminal](Trading-Terminal.md) specific.*

1. `symbols`: array of symbols to be updated rarely (suggested frequency is once per minute). These symbols are in the watch list but they are not visible at the moment.
1. `fastSymbols`: array of symbols to be updated quite frequently (once in 10 seconds or more often)
1. `onRealtimeCallback`: function(array of `data`)
    1. `data`: [symbol quote data](Quotes.md#symbol-quote-data)
1. `listenerGUID`: unique identifier of the listener

Trading Terminal calls this function when it wants to receive realtime quotes for a symbol.

Chart expects you will call `onRealtimeCallback` every time you want to update quotes.

### unsubscribeQuotes(listenerGUID)

:chart: *[Trading Terminal](Trading-Terminal.md) specific.*

1. `listenerGUID`: unique identifier of the listener

Trading Terminal calls this function when is doesn't want to receive updates for this listener any more.

`listenerGUID` will be the same object which Library passed to `subscribeQuotes` before.

### subscribeDepth(symbolInfo, callback): string

:chart: *[Trading Terminal](Trading-Terminal.md) specific.*

1. `symbolInfo`: [SymbolInfo](Symbology.md#symbolinfo-structure) object
1. `callback`: function(depth)
    1. `depth`: object `{snapshot, asks, bids}`
        * `snapshot`: Boolean - if `true` `asks` and `bids` have full set of depth, otherwise they contain only updated levels.
        * `asks`: Array of `{price, volume}`
        * `bids`: Array of `{price, volume}`

Trading Terminal calls this function when it wants to receive realtime level 2 (DOM) for a symbol. Chart expects you will call `callback` every time you want to update depth data.

This method should return unique identified (`subscriberUID`) that will be used to unsubscribe data.

### unsubscribeDepth(subscriberUID)

:chart: *[Trading Terminal](Trading-Terminal.md) specific.*

1. `subscriberUID`: String

Trading Terminal calls this function when is doesn't want to receive updates for this listener any more.

`subscriberUID` will be the same object which you have returned from `subscribeDepth`.
