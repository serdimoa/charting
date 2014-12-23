Here is a list of supported widget's methods. You can call them using widget object returned to you by widget's constructor.

####remove()
Removes chart widget from your page.

####onChartReady(callback)
1. `callback`: function()

The Charting Library will call the callback provided once when chart is initialized and ready. You can safely call all other methods from this moment.

####onSymbolChange(callback)
1. `callback`: function(symbolData)
  1.  `symbolData`: object `{name, exchange, description, type, interval}

The Charting Library will call the callback provided every time the main series symbol changes. New symbol info will be passed as argument.

####onIntervalChange(callback)
1. `callback`: function(interval)
  1. interval: string

The Charting Library will call the callback provided every time the main series interval changes. New interval will be passed as argument.

####setSymbol(symbol, interval, callback)
1. `symbol`: string
2. `interval`: string
3. `callback`: function()

Makes the chart to change its symbol and resolution. Callback is called after new symbol's data arrived.

####symbolInterval(callback)
1. `callback`: function(result)
  1. `result`: object `{symbol, interval}`

Charting Library will call your callback passes some data about chart's symbol and interval.

####onRealtimeTick(callback)
1. `callback`: function(barData)
  1. `barData`: object `{time, open, high, low, close, volume}`

The Charting Library will call the callback provided every time the new bar update comes.

####createShape(point, options)
1. `point`: object `{time, [price], [channel]}`
  1. `time`: unix time. The only mandatory argument.
  2. `price`: If you specify `price`, then your icon will be placed on its level. If you do not, then the icon sticks to bar at respective time.
  3. `channel`: The price level to stick to is specified by `channel` argument (`open`, `high`, `low`, `close`). If no channel is specified, 'open' is a default.
2. `options`: object `{shape, [text], [lock], [overrides]}`
  1. `shape` may be one of the ['arrow_up', 'arrow_down', 'flag', 'vertical_line']. 'flag' is the default value.
  2. `text` is an optional argument. It's the text that will be assigned to shape if it can contain a text (for now, it's only 'arrow_up' and 'arrow_down').
  3. `lock` shows whether a user will be able to remove/change/hide your shape or not.
  4. `overrides` (since version `1.2`). It is an object containing properties you'd like to set for your new shape.

Supported overrides:

* Shape: vertical_line
  1. `linecolor` [#80CCDB]
  2. `linewidth` [1.0]
  3. `linestyle` [1]
  4. `showTime` [true]
* Shape: arrow_up, arrow_down
  1. `color` [#787878]
  2. `text` [""]
  3. `fontsize` [20]
  4. `font` ["Verdana"]
* Shape: flag
  1. `color` [#FF0000]

This call creates a shape at specified point on main series.

####createVerticalLine(point, options)
1. `point`: object `{time}`
2. `options`: obejct `{lock}`

This function is a synonym for `createShape` with shape = 'vertical_line'. It is treated as **obsolete**.

####createStudy(name, forceOverlay, lock, inputs)
1. `name`: string, a name of an indicator as you can see it in `Indicators` widget
2. `forceOverlay`: forces the Charting Library to place the created study on main pane
3. `lock`: boolean, shows whether a user will be able to remove/change/hide your study or not
4. `inputs`: (since version `1.2`) an array of study inputs. This array is expected to contain just inputs values in the same order they are printed in study's properties page.

Creates the study on a main symbol. Examples: 
  * `createStudy('MACD', false, false, [14, 30, 9])`
  * `createStudy('Moving Average Exponential', false, false, [26])`

####save(callback)
1. `callback`: function(object)

Saves the chart state to JS object. Charting Library will call your callback and pass the state object as argument. This call is a part of low-level [[save/load API|Saving-and-Loading-Charts]].

####load(state)
1. `state`: object
Loads the chart from state object. This call is a part of low-level [[save/load API|Saving-and-Loading-Charts]].

####onAutoSaveNeeded(callback)
1. `callback`: function()

The Library will call the callback provided every time when user changes the chart. `Chart change` means any user action that can be undone. The callback will not be called more than once in five seconds.

####onMarkClick(callback)
1. `callback`: function(markId)
  1. markId: object

The Library will call the callback provided every time when user clicks a [[mark on bar|Marks-On-Bars]]. Mark ID will be passed as an argument.

####onGrayedObjectClicked(callback)
1. `callback`: function(subject)
  1. `subject`: object `{type, name}`
    1. `type`: `drawing` | `study`
    2. `name`: string, name of a subject which was clicked

The Library will call the callback provided every time when user clicks on grayed out object. Example:

```
new TradingView.widget({
    drawings_access: {
        type: "black",
        tools: [
            { name: "Regression Trend" },
            { name: "Trend Angle", grayed: true },
        ]
    },
    studies_access: {
        type: "black",
        tools: [
            { name: "Aroon" },
            { name: "Balance of Power", grayed: true },
        ]
    },
    <...> // other widget settings
});

widget.onChartReady(function() {
    widget.onGrayedObjectClicked(function(data) {
        // this fucntion will be called when one tries to
        // create Balance Of Power study or Trend Angle shape

        alert(data.name + " is grayed out!");
    })
});

```