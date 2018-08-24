Here is a list of methods supported by the chart.

**Before version 1.4.** You can call these methods using widget object returned to you by widget's constructor.

**Starting from version 1.5.** You can call these methods using chart object returned to you by widget's methods [chart(index)](Widget-Methods#chart-chartindex) or [activeChart()](Widget-Methods#chart-activechart).

## Methods

* [Subscribing To Chart Events](#subscribing-to-chart-events)
  * [onDataLoaded()](#ondataloaded)
  * [onSymbolChanged()](#onsymbolchanged)
  * [onIntervalChanged()](#onintervalchanged)
  * [dataReady(callback)](#datareadycallback)
  * [crossHairMoved(callback)](#crosshairmovedcallback)
  * [onVisibleRangeChanged()](#onvisiblerangechanged)
* [Chart Actions](#chart-actions)
  * [setVisibleRange(range, callback)](#setvisiblerangerange-callback)
  * [setSymbol(symbol, callback)](#setsymbolsymbol-callback)
  * [setResolution(resolution, callback)](#setresolutionresolution-callback)
  * [resetData()](#resetdata)
  * [executeActionById(action)](#executeactionbyidactionid)
  * [getCheckableActionState(action)](#getcheckableactionstateactionid)
  * [refreshMarks()](#refreshmarks)
  * [clearMarks()](#clearmarks)
  * [setChartType(type)](#setcharttypetype)
  * [setTimezone(timezone)](#settimezonetimezone)
* [Studies And Shapes](#studies-and-shapes)
  * [getAllShapes()](#getallshapes)
  * [getAllStudies()](#getallstudies)
  * [setEntityVisibility(id, isVisible)](#setentityvisibilityid-isvisible) [obsolete]
  * [createStudy(name, forceOverlay, lock, inputs, callback, overrides, options)](#createstudyname-forceoverlay-lock-inputs-callback-overrides-options)
  * [getStudyById(entityId)](#getstudybyidentityid)
  * [createShape(point, options)](#createshapepoint-options)
  * [createMultipointShape(points, options)](#createmultipointshapepoints-options)
  * [getShapeById(entityId)](#getshapebyidentityid)
  * [removeEntity(entityId)](#removeentityentityid)
  * [removeAllShapes()](#removeallshapes)
  * [removeAllStudies()](#removeallstudies)
  * [getPanes()](#getpanes)
* [Study Templates](#study-templates)
  * [createStudyTemplate(options)](#createstudytemplateoptions)
  * [applyStudyTemplate(template)](#applystudytemplatetemplate)
* [Trading Primitives](#trading-primitives)
  * [createOrderLine()](#createorderlineoptions)
  * [createPositionLine()](#createpositionlineoptions)
  * [createExecutionShape()](#createexecutionshapeoptions)
* [Getters](#getters)
  * [symbol()](#symbol)
  * [symbolExt()](#symbolExt)
  * [resolution()](#resolution)
  * [getVisibleRange()](#getvisiblerange)
  * [getVisiblePriceRange()](#getvisiblepricerange)
  * [priceFormatter()](#priceformatter)
  * [chartType()](#charttype)
* [Other](#other)
  * [exportData(options)](#exportdataoptions)

## Subscribing To Chart Events

### onDataLoaded()

You can subscribe using [Subscription](Subscription) object returned by this function to be notified when new history bars are loaded. You can also use the same object to unsubscribe from the event.

### onSymbolChanged()

You can subscribe using [Subscription](Subscription) object returned by this function to be notified when the symbol is changed. You can also use the same object to unsubscribe from the event.

### onIntervalChanged()

You can subscribe using [Subscription](Subscription) object returned by this function to be notified when the interval is changed. You can also use the same object to unsubscribe from the event.
When the event is fired it will provide the following arguments:

1. `interval`: new interval
1. `timeframeParameters`: object with the only field `timeframe`.

    It contains a timeframe if the interval is changed when the user clicks on the timeframe panel.

    Otherwise `timeframe` is `undefined` and you can change it to display a certain range of bars. Valid timeframe is a number with letter `D` for days and `M` for months.

Example:

```javascript
widget.chart().onIntervalChanged().subscribe(null, function(interval, obj) {
    obj.timeframe = "12M";
})
```

### dataReady(callback)

1. `callback`: function(interval)

The Charting Library will immediately call the callback function if bars are already loaded or when the bars are received.
The function returns `true` if bars are already loaded and `false` otherwise.

### crossHairMoved(callback)

*Since version 1.5.*

1. `callback`: function({time, price})

The Charting Library will call the callback function every time the crosshair position is changed.

### onVisibleRangeChanged()

*Since version 1.13.*

You can subscribe using [Subscription](Subscription) object returned by this function to be notified when visible time range is changed. You can also use the same object to unsubscribe from the event.

## Chart Actions

### setVisibleRange(range, callback)

1. `range`: object, `{from to}`
    * `from`, `to`: unix timestamps, UTC
1. `callback`: `function()`. The Library will call it after it's done with the viewport setup.

Forces the chart to adjust its parameters (scroll, scale) to make the selected time period fit the widget.
Neither `from`, nor `to` can be set to a future date. This method was introduced in version `1.2`.

### setSymbol(symbol, callback)

1. `symbol`: string
1. `callback`: function()

Makes the chart change its symbol. Callback function is called once the data for the new symbol is loaded.

### setResolution(resolution, callback)

1. `resolution`: string. Format is described in another [article](Resolution).
1. `callback`: function()

Makes the chart change its resolution. Callback function is called once new data is loaded.

### resetData()

Makes the chart re-request data from the data feed. The function is often called when chart's data has changed.

Before calling this function you should call [onResetCacheNeededCallback](JS-Api#subscribebarssymbolinfo-resolution-onrealtimecallback-subscriberuid-onresetcacheneededcallback).

### executeActionById(actionId)

*Starting from version 1.3.*

1. `actionId`: string

Executes an action according to its id.

**Shows a dialog:**

* `chartProperties`
* `compareOrAdd`
* `scalesProperties`
* `tmzProperties`
* `paneObjectTree`
* `insertIndicator`
* `symbolSearch`
* `changeInterval`

**Other actions:**

* `timeScaleReset`
* `chartReset`
* `seriesHide`
* `studyHide`
* `lineToggleLock`
* `lineHide`
* `showLeftAxis`
* `showRightAxis`
* `scaleSeriesOnly`
* `drawingToolbarAction`
* `magnetAction`
* `stayInDrawingModeAction`
* `hideAllMarks`
* `showCountdown`
* `showSeriesLastValue`
* `showSymbolLabelsAction`
* `showStudyLastValue`
* `showStudyPlotNamesAction`
* `undo`
* `redo`
* `paneRemoveAllStudiesDrawingTools`

Examples:

```javascript
// < ... >
widget.chart().executeActionById("undo");
// < ... >
widget.chart().executeActionById("drawingToolbarAction"); // hides or shows the drawing toolbar
// < ... >
```

### getCheckableActionState(actionId)

*Starting from version 1.7.*

1. `actionId`: string

Get a checkable action state (e.g. `stayInDrawingModeAction`, `magnetAction`) according to its ID (see the IDs of actions above)

### refreshMarks()

When you call this method the Library requests visible marks once again.

### clearMarks()

When you call this method the Library removes all visible marks.

### setChartType(type)

1. `type`: number

Sets the main series style.

```javascript
STYLE_BARS = 0;
STYLE_CANDLES = 1;
STYLE_LINE = 2;
STYLE_AREA = 3;
STYLE_HEIKEN_ASHI = 8;
STYLE_HOLLOW_CANDLES = 9;
STYLE_BASELINE = 10;

STYLE_RENKO* = 4;
STYLE_KAGI* = 5;
STYLE_PNF* = 6;
STYLE_PB* = 7;
```

*- :chart: available in Trading Terminal

### closePopupsAndDialogs()

When you call this method a context menu or a dialog closes (if applicable).

### setTimezone(timezone)

1. `timezone`: string

See [timezone](Widget-Constructor#timezone) for more information.

Example:

```javascript
widget.activeChart().setTimezone('Asia/Singapore');
```

Makes the chart change its timezone.

## Studies And Shapes

### getAllShapes()

Returns an array of all created shape objects. Each object has the following fields:

* `id`: id of a shape
* `name`: name of a shape

### getAllStudies()

Returns an array of all created shape objects. Each object has the following fields:

* `id`: id of a study
* `name`: name of a study

### setEntityVisibility(id, isVisible)

Sets visibility of an entity with a passed ID.

**Deprecated**: Use a shape/study API instead (`getShapeById`/`getStudyById`). This is going to be removed in future releases.

### createStudy(name, forceOverlay, lock, inputs, callback, overrides, options)

1. `name`: string, name of an indicator as shown in the `Indicators` widget
1. `forceOverlay`: forces the Charting Library to place the created study on the main pane
1. `lock`: boolean, shows whether a user will be able to remove/change/hide the study or not
1. `inputs`: (starting from version `1.2`) an array of study inputs. This array is expected to contain input values in the same order as in the study properties dialog.
1. `callback`: function(`entityId`)
1. `overrides`: (starting from version `1.2`) an object [containing properties](Studies-Overrides) you'd like to set for your new study. Note that you should not specify the study name. Start a property path with a plot name.
1. `options`: object with the the following keys:
    * `checkLimit` - if it is `true` then the study limit dialog will be shown if the limit is exceeded.
    * `priceScale` - preferred price scale for the study. Possible values are:
        * `left` - attach the study to the left price scale
        * `right` - attach the study to the right price scale
        * `no-scale` - do not attach the study to any price scale. The study will be added in 'No Scale' mode
        * `as-series` - attach the study to the price scale where the main series is attached (it is only applicable the study is added to the pane with the main series)

**Starting from v 1.12 the function returns the result immediately. Callback is kept to maintain compatibility.**

Creates a study on the main symbol. Here are the examples:

* `createStudy('MACD', false, false, [14, 30, "close", 9])`
* `createStudy('Moving Average Exponential', false, false, [26])`
* `createStudy('Stochastic', false, false, [26], null, {"%d.color" : "#FF0000"})`
* `chart.createStudy('Moving Average', false, false, [26], null, {'Plot.linewidth': 10})`

**Remark**: The `Compare` study has 2 inputs: `[dataSource, symbol]`. Supported `dataSource` values are: `["close", "high", "low", "open"]`.

**Remark 2**: You use `Overlay` study when you choose to `Add` series on the chart. This study has a single input -- `symbol`. Here is an example of adding a symbol:

```javascript
    widget.chart().createStudy('Overlay', false, false, ['AAPL']);
```

**Remark 3**: You also use the `Compare` study when you choose to compare different financial instruments. This study has two inputs -- `source` and `symbol`. Here is an example:

```javascript
    widget.chart().createStudy('Compare', false, false, ["open", 'AAPL']);
```

### getStudyById(entityId)

1. `entityId`: object. Value that is returned when a study is created via API.

Returns an instance of the [StudyApi](Study-Api) that allows you to interact with the study.

### createShape(point, options)

1. `point`: object `{time, [price], [channel]}`
    * `time`: unix time. It's the only mandatory key in this function argument.
    * `price`: If you specify `price`, then the shape will be placed at the same price level.
        If not, then the shape will be placed close to the bar according the `channel` value.
    * `channel`: If the price is not set then `channel` value defines where the shape is placed relative to the bar. Possible values are `open`, `high`, `low`, `close`.
        If no channel is specified then 'open' is a default value.
1. `options`: object `{shape, [text], [lock], [overrides]}`
    * `shape` could be one of the following: `arrow_up`, `arrow_down`, `flag`, `vertical_line`, `horizontal_line`.
        `flag` is the default value.
    * `text` is an optional argument. It's the text that will be included in the shape if it's supported.
    * `lock` shows whether a user will be able to remove/change/hide the shape or not.
    * `disableSelection` prevents selecting of the shape
    * `disableSave` prevents saving the shape on the chart
    * `disableUndo` prevents adding of the action to the undo stack
    * `overrides` is an object containing properties you'd like to set for your new shape.
    * `zOrder` can have the following values `top`, `bottom`.
        `top` places the line tool on top of all other chart objects while `bottom` places the line tool behind all other chart objects.
        If not specified the line tool is placed on top of all existing chart objects.
    * `showInObjectsTree`: Displays the shape in the Objects Tree dialog. The default value is `true`.

The function returns `entityId` - unique ID of the shape if the creation was successful and `null` if it wasn't.

This call creates a shape at a specific point on the chart provided that it's within the main series area.

### createMultipointShape(points, options)

1. `points`: is an array of object with the following keys `[{time, [price], [channel]},...]`
    * `time`: unix time. It's the only mandatory key in this function argument.
    * `price`: If you specify `price`, then the shape will be placed at the same price level.
        If not, then the shape will be placed close to the bar according the `channel` value.
    * `channel`: If the price is not set then `channel` value defines where the shape is placed relative to the bar. Possible values are `open`, `high`, `low`, `close`.
        If no channel is specified, 'open' is a default value.
1. `options`: object `{shape, [text], [lock], [overrides]}`
    * `shape` may be one of the [identifiers](Shapes-and-Overrides)
    * `text` is an optional argument. It's the text that will be included in the shape if it's supported.
    * `lock` shows whether a user will be able to remove/change/hide the shape or not.
    * `disableSelection` prevents selecting of the shape
    * `disableSave` prevents saving the shape on the chart
    * `disableUndo` prevents adding of the action to the undo stack
    * `overrides` is an object containing properties you'd like to set for your new shape.
    * `zOrder` can have the following values `top`, `bottom`.
        `top` places the line tool on top of all other chart objects while `bottom` places the line tool behind all other chart objects.
        If not specified the line tool is placed on top of all existing chart objects.
    * `showInObjectsTree`: Displays the shape in the Objects Tree dialog. The default value is `true`.

The function returns `entityId` - unique ID of the shape if the creation was successful and `null` if it wasn't.

Check out [Shapes and Overrides](Shapes-and-Overrides) for more information.

This call creates a shape at a specific point on the chart provided that it's within the main series area.

### getShapeById(entityId)

1. `entityId`: object. The value that is returned when a shape is created via API

Returns an instance of the [ShapeApi](Shape-Api) that allows you to interact with the shape.

### removeEntity(entityId)

1. `entityId`: object. It's the value that was returned when the entity (shape or study) was created.

Removes the specified entity.

### removeAllShapes()

Removes all the shapes from the chart.

### removeAllStudies()

Removed all the studies from the chart.

### getPanes()

Returns an array of instances of the [PaneApi](Pane-Api) that allows you to interact with the panes.

## Study Templates

### createStudyTemplate(options)

1. `options`: object `{saveInterval}`
    * `saveInterval`: boolean

Saves the study template to JS object. Charting Library will call your callback function and pass the state object as an argument.

This call is a part of low-level [save/load API](Saving-and-Loading-Charts).

### applyStudyTemplate(template)

1. `template`: object

Loads the study template from the `template` object.

This call is a part of low-level [save/load API](Saving-and-Loading-Charts).

## Trading Primitives

### createOrderLine(options)

1. `options` is an object with one possible key - `disableUndo` which can be `true` or `false`.
    For compatibility reasons the default value is set to `false`.

Creates a new trading order on the chart and returns an API-object that you can use to adjust its properties and behavior.

It is strongly recommended to read [this article](Trading-Primitives) before using this call.

API object methods:

* `remove()`: Removes the position from the chart. You can’t this API-object after the call.
* `onModify(callback)` / `onModify(data, callback)`
* `onMove(callback)` / `onMove(data, callback)`

API object has a set of properties listed below. Each property should be used through respective accessors.
For example, if you wish to work with the `Extend Left` property, then use `getExtendLeft()` of `setExtendLeft()` methods.

**General properties**:

Property|Type|Supported Values|Default Value
---|---|---|---
Price|Double|Double|0.0
Text|String|String|""
Tooltip|String|String|""
Quantity|String|String|""
Editable|Boolean|Boolean|true

**Horizontal line properties**:

Property|Type|Supported Values|Default Value
---|---|---|---
Extend Left|Boolean|"inherit" or Boolean|True
Line Length|Integer|"inherit" or 0 .. 100|0
Line Style|Integer|"inherit" or 0 .. 2|2
Line Width|Integer|"inherit" or 1 .. 4|1

**Fonts**:

Property|Type|Default Value
---|---|---
Body Font|String|"bold 7pt Verdana"
Quantity Font|String|"bold 7pt Verdana"

**Colors**:

Property|Type|Default Value
---|---|---
Line Color|String|"rgb(255, 0, 0)"
Body Border Color|String|"rgb(255, 0, 0)"
Body Background Color|String|"rgba(255, 255, 255, 0.75)"
Body Text Color|String|"rgb(255, 0, 0)"
Quantity Border Color|String|"rgb(255, 0, 0)"
Quantity Background Color|String|"rgba(255, 0, 0, 0.75)"
Quantity Text Color|String|"rgb(255, 255, 255)"
Cancel Button Border Color|String|"rgb(255, 0, 0)"
Cancel Button Background Color|String|"rgba(255, 255, 255, 0.75)"
Cancel Button Icon Color|String|"rgb(255, 0, 0)"

Example:

```javascript
widget.chart().createOrderLine()
    .onMove(function() {
        this.setText("onMove called");
    })
    .onModify("onModify called", function(text) {
        this.setText(text);
    })
    .onCancel("onCancel called", function(text) {
        this.setText(text);
    })
    .setText("STOP: 73.5 (5,64%)")
    .setQuantity("2");
```

### createPositionLine(options)

1. `options` is an object with one possible key - `disableUndo` which can be `true` or `false`.
    For compatibility reasons the default value is set to `false`.

Creates a new trading position on the chart and returns an API-object that you can use to adjust its properties and behavior.

It is strongly recommended to read [this article](Trading-Primitives) before using this call.

API object methods:

* `remove()`: Removes the position from the chart. You can’t use this API-object after the call.
* `onClose(callback)` / `onClose(data, callback)`
* `onModify(callback)` / `onModify(data, callback)`
* `onReverse(callback)` / `onReverse(data, callback)`

API object has a set of properties listed below. Each property should be used through respective accessors.
For example, if you wish to work with `Extend Left` property, use `getExtendLeft()` of `setExtendLeft()` methods.

**General properties**:

Property|Type|Supported Values|Default Value
---|---|---|---
Price|Double|Double|0.0
Text|String|String|""
Tooltip|String|String|""
Quantity|String|String|""

**Horizontal line properties**:

Property|Type|Supported Values|Default Value
---|---|---|---
Extend Left|Boolean|"inherit" or Boolean|True
Line Length|Integer|"inherit" or 0 .. 100|0
Line Style|Integer|"inherit" or 0 .. 2|2
Line Width|Integer|"inherit" or 1 .. 4|1

**Fonts**:

Property|Type|Default Value
---|---|---
Body Font|String|"bold 7pt Verdana"
Quantity Font|String|"bold 7pt Verdana"

**Colors**:

Property|Type|Default Value
---|---|---
Line Color|String|"rgb(0, 113, 224)"
Body Border Color|String|"rgb(0, 113, 224)"
Body Background Color|String|"rgba(255, 255, 255, 0.75)"
Body Text Color|String|"rgb(0, 113, 224)"
Quantity Border Color|String|"rgb(0, 113, 224)"
Quantity Background Color|String|"rgba(0, 113, 224, 0.75)"
Quantity Text Color|String|"rgb(255, 255, 255)"
Reverse Button Border Color|String|"rgb(0, 113, 224)"
Reverse Button Background Color|String|"rgba(255, 255, 255, 0.75)"
Reverse Button Icon Color|String|"rgb(0, 113, 224)"
Close Button Border Color|String|"rgb(0, 113, 224)"
Close Button Background Color|String|"rgba(255, 255, 255, 0.75)"
Close Button Icon Color|String|"rgb(0, 113, 224)"

Example:

```javascript
widget.chart().createPositionLine()
    .onModify(function() {
        this.setText("onModify called");
    })
    .onReverse("onReverse called", function(text) {
        this.setText(text);
    })
    .onClose("onClose called", function(text) {
        this.setText(text);
    })
    .setText("PROFIT: 71.1 (3.31%)")
    .setQuantity("8.235")
    .setPrice(15.5)
    .setExtendLeft(false)
    .setLineStyle(0)
    .setLineLength(25);
```

### createExecutionShape(options)

1. `options` is an object with one possible key - `disableUndo` which can be `true` or `false`.
    For compatibility reasons the default value is set to `false`.

Creates a new trade execution on the chart and returns an API-object that you can use to control the execution properties.

It is strongly recommended to read [this article](Trading-Primitives) before using this call.

API object has a set of properties listed below. Each property should be used through respective accessors.
For example, if you wish to work with `Extend Left` property, then use `getExtendLeft()` of `setExtendLeft()` methods.

API object methods:

* `remove()`: Removes the execution shape from the chart. You can’t use this API-object after the call.

**General properties**:

Property|Type|Supported Values|Default Value
---|---|---|---
Price|Double|Double|0.0
Time|Integer|Unix time|0
Direction|String|"buy" or "sell"|"buy"
Text|String|String|"execution"
Tooltip|String|String|
Arrow Height|Integer|Integer|8
Arrow Spacing|Integer|Integer|1

**Fonts**:

Property|Type|Default Value
---|---|---
Font|String|"8pt Verdana"

**Colors**:

Property|Type|Default Value
---|---|---
Text Color|String|"rgb(0, 0, 0)""
Arrow Color|String|"rgba(0, 0, 255)"

Example:

```javascript
widget.chart().createExecutionShape()
    .setText("@1,320.75 Limit Buy 1")
    .setTooltip("@1,320.75 Limit Buy 1")
    .setTextColor("rgba(0,255,0,0.5)")
    .setArrowColor("#0F0")
    .setDirection("buy")
    .setTime(1413559061758)
    .setPrice(15.5);
```

## Getters

### symbol()

Returns the current symbol of the chart.

### symbolExt()

Returns the current symbol information of the chart. The object has the following fields:

* `symbol`: is the same as the result of [symbol()](#symbol) method
* `full_name`: the full name of the symbol
* `exchange`: the exchange of the symbol
* `description`: the description of the symbol
* `type`: the type of the symbol

### resolution()

Returns the chart's time interval. The format is described in this [article](Resolution).

### getVisibleRange()

Returns the object `{from, to}`. `from` and `to` are Unix timestamps **in the timezone of the chart**.

### getVisiblePriceRange()

*Starting from V 1.7.*

Returns the object `{from, to}`. `from` and `to` are boundaries of the price scale visible range in main series area.

### priceFormatter()

Returns the object with `format` function that you can use to format the prices.

### chartType()

Returns the main series style type.

## Other

### exportData(options)

*Starting from version 1.13.*

1. `options` (optional) is an object, which can contain the following properties:
    * `from` (`number`) - date of the first exporting bar (UNIX timestamp in seconds).
        By default the time of the leftmost loaded bar is used.
    * `to` (`number`) - date of the last exporting bar (UNIX timestamp in seconds).
        By default the time of the rightmost (real-time) bar is used.
    * `includeTime` (`boolean`, default `true`) - defines whether each item of the exported data should contain time.
    * `includeSeries` (`boolean`, default `true`) - defines whether the exported data should contain the main series (open, high, low, close).
    * `includedStudies` - which studies should be included in the exported data
        (by default, the value is `'all'` which means that all studies are included, but if you want to export only some of them then you can assign an array of [studies' ids](#getallstudies)).

Exports data from the chart, returns a Promise object. This method doesn't load data. The result has the following structure:

* `schema` is an array of field descriptors, each descriptor might be one the following types:
  * `TimeFieldDescriptor` - description of the time field. It contains only one field - `type` with the `'time'` value.
  * `SeriesFieldDescriptor` - description of a series field. It contains the following fields:
    * `type` (`'value'`)
    * `sourceType` (`'series'`)
    * `plotTitle` (`string`) - the name of the plot (open, high, low, close).
  * `StudyFieldDescriptor` - description of a study field. It contains the following fields:
    * `type` (`'value'`)
    * `sourceType` (`'study'`)
    * `sourceId` (`string`) - id of the study
    * `sourceTitle` (`string`) - title of the study
    * `plotTitle` (`string`) - title of the plot

* `data` is an array of [Float64Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float64Array)s.
   Each `Float64Array` array has the same length as `schema` array and represents the associated field's item.

**Examples:**

1. `chart.exportData({ includeTime: false, includeSeries: true, includedStudies: [] })` - to export series' data only.
1. `chart.exportData({ includeTime: true, includeSeries: true, includedStudies: [] })` - to export series' data with times.
1. `chart.exportData({ includeTime: false, includeSeries: false, includedStudies: ['STUDY_ID'] })` - to export data for the study with the id `STUDY_ID`.
1. `chart.exportData({ includeTime: true, includeSeries: true, includedStudies: 'all' })` - to export all available data from the chart.
1. `chart.exportData({ includeTime: false, includeSeries: true, to: Date.UTC(2018, 0, 1) / 1000 })` - to export series' data before `2018-01-01`.
1. `chart.exportData({ includeTime: false, includeSeries: true, from: Date.UTC(2018, 0, 1) / 1000 })` - to export series' data in the range before `2018-01-01`.
1. `chart.exportData({ includeTime: false, includeSeries: true, from: Date.UTC(2018, 0, 1) / 1000, to: Date.UTC(2018, 1, 1) / 1000 })` - to export series' data in the range between `2018-01-01` and `2018-02-01`.

## See Also

* [Widget Methods](Widget-Methods)
* [Customization Overview](Customization-Overview)
* [Widget Constructor](Widget-Constructor)
* [Saving and Loading Charts](Saving-and-Loading-Charts)
* [Overriding Studies' Defaults](Studies-Overrides)
* [Overriding Chart's Defaults](Overrides)
