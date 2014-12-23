#Overview

To feed your data to the charts using JS API, create an object that has a specific public interface. It’s rather compact; your object needs a few functions. Data caching (history & symbol info) is implemented in Charting Library. When you create an object implementing described interface, just pass it to Library widget constructor through `datafeed `argument.

#Methods

1. setup
2. searchSymbolsByName
3. resolveSymbol
4. getBars
5. subscribeBars
6. unsubscribeBars
7. calculateHistoryDepth
8. getMarks
9. getQuotes

###setup(reserved, callback)
1. `reserved` is not used now
2. `callback`: function(configurationData)
  1. `configurationData`: object (see below)

This call is intended to provide the object filled with configuration data. Charting Library expects you will call callback and pass your datafeed `configurationData` as an argument. Configuration data is an object; for now, following properties are supported:

#####exchanges
An array of exchange descriptors. Exchange descriptor is an object `{value, name, desc}`. `value` will be passed as `exchange` argument  to searchSymbolsByName (see below).

`exchanges` = []  leads to exchanges filter absence in Symbol Search list. Use `value` = "" if you want to create wildcard filter (all exchanges).

#####symbols_types
An array of filter descriptors. Filter descriptor is an object `{name, value}`. `value` will be passed as `symbolType` argument  to searchSymbolsByName. 

`symbolsTypes` = []  leads to types filter absence in Symbol Search list. Use `value` = "" if you want to create wildcard filter (all types)

#####supported_resolutions
An array of supported resolutions. Resolution may be a number or a string. If the resolution is a number, it is treated as minutes count. Strings may be "*D", "*W", "*M" (* means any number).

'resolutions'=undefined or [] leads to resolutons widget having its default content (see <http://tradingview.com/e/> ). Example: `[1, 15, 240, "D", "6M"]` will give you "1 minute, 15 minutes, 4 hours, 1 day, 6 months" in resolution widget.

#####supports_marks
Boolean showing whether your datafeed supports marks on bars or not. 

###searchSymbolsByName(userInput, exchange, symbolType, onResultReadyCallback)
1. `userInput`: string. It is text entered by user in symbol search field
2. `exchange`: string. The requested exchange (chosen by user). Empty value means no filter was specified.
3. `symbolType`: string. The requested symbol type: index, stock, forex e.t.c. (chosen by user). Empty value means no filter was specified.
4. `onResultReadyCallback`: function(result)
  1. `result`: ```javascript
[
    {
        "symbol": <short symbol name>,
        "full_name": <full symbol name -- e.g., BTCE:BTCUSD>,
        "description": <symbol description>,
        "exchange": <symbol exchange name>,
            "ticker": <symbol ticker name>,
        "type": "stock" | "futures" | "bitcoin" | "forex" | "index"
    }, {
        //    .....
    }
]
```

