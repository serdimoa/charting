Here is a list of supported widget's methods. You can call them using widget object returned to you by widget's constructor.

####remove()
Removes chart widget from your page.

####onChartReady(callback)
`callback`: function()

The Charting Library will call the callback provided once when chart is initialized and ready. You can safely call all other methods from this moment.

####onSymbolChange(callback)
1. `callback`: function(symbolData)

The Charting Library will call the callback provided every time the main series symbol changes. New symbol info will be passed as argument. `symbolData` format is `{name, exchange, description, type, interval}`

####onIntervalChange(callback)
1. `callback`: function(interval)

The Charting Library will call the callback provided every time the main series interval changes. New interval will be passed as argument.

####setSymbol(symbol, interval, callback)
1. `symbol`: string
2. `interval`: string
3. `callback`: function()
Makes the chart to change its symbol and resolution. Callback is called after new symbol's data arrived.

####symbolInterval(callback)
1. `callback`: function(result)
Calls callback and passes object `{symbol, interval}` as argument.

####onRealtimeTick(callback)
1. `callback`: function(barData)
The Charting Library will call the callback provided every time the new bar update comes. `barData` is an object `{time, open, high, low, close, volume}`.

####createShape(point, options)
1. `point`: object `{time, [price], [channel]}`
  1. `time`: unix time. The only mandatory argument.
  2. `price`: If you specify `price`, then your icon will be placed on its level. If you do not, then the icon sticks to bar at respective time.
  3. `channel`: The price level to stick to is specified by `channel` argument (`open`, `high`, `low`, `close`). If no channel is specified, 'open' is a default.
2. `options`: object `{shape, [text], [lock], [overrides]}`
  1. `shape` may be one of the ['arrow_up', 'arrow_down', 'flag', 'vertical_line']. 'flag' is the default value.
  2. `text` is an optional argument. It's the text that will be assigned to shape if it can contain a text (for now, it's only 'arrow_up' and 'arrow_down').
  3. `lock` shows whether a user will be able to remove/change/hide your shape or not.
  4. `overrides` (till `version: 1.2`). It is an object containing properties you'd like to set for your new shape. Supported properties:



