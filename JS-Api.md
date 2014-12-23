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
  1. `configurationData`: object `{exchanges, symbols_types, supported_resolutions, supports_marks}`
    1. `exchanges`: An array of exchange descriptors. Exchange descriptor is an object `{value, name, desc}`. `value` will be passed as `exchange` argument  to searchSymbolsByName (see below). `exchanges` = []  leads to exchanges filter absence in Symbol Search list.


This call is intended to provide the object filled with configuration data. Charting Library expects you will call callback and pass your datafeed `configurationData` as an argument.