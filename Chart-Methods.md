Here is a list of supported chart's methods.

**Before 1.4 version.** You can call these methods using widget object returned to you by widget's constructor.

**Since 1.5 version.** You can call these methods using chart object returned to you by widget's methods [chart(index)](Widget-Methods.md#chart-chartindex) or [activeChart()](Widget-Methods.md#chart-activechart).

## Methods

* [Subscribing To Chart Events](#subscribing-to-chart-events)
  * [onDataLoaded()](#ondataloaded)
  * [onSymbolChanged()](#onsymbolchanged)
  * [onIntervalChanged()](#onintervalchanged)
  * [dataReady(callback)](#datareadycallback)
  * [crossHairMoved(callback)](#crosshairmovedcallback)
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

## Subscribing To Chart Events

### onDataLoaded()

You can subscribe using [Subscription](Subscription.md) object returned by this function to be notified when new history bars are loaded and unsubscribe from the event.

### onSymbolChanged()

You can subscribe using [Subscription](Subscription.md) object returned by this function to be notified when the symbol is changed and unsubscribe from the event.

### onIntervalChanged()

You can subscribe using [Subscription](Subscription.md) object returned by this function to be notified when the interval is changed and unsubscribe from the event.
When the event is fired it will provide the following arguments:

1. `interval`: new interval
1. `timeframeParameters`: object with the only field `timeframe`.

    It contains a timeframe if the interval is changed as a result of a user click on a timeframe panel.

    Otherwise `timeframe` is `undefined` and you can change it to display a certain range of bars. Valid timeframe is a number with letter `D` for days and `M` for months.

Example:

```javascript
widget.chart().onIntervalChanged().subscribe(null, function(interval, obj) {
    obj.timeframe = "12M";
})
```

### dataReady(callback)

1. `callback`: function(interval)

The Charting Library will call the callback provided immediately if bars are already loaded or when the bars are received.
The function returns `true` if bars are already loaded and `false` otherwise.

### crossHairMoved(callback)

*Since version 1.5.*

1. `callback`: function({time, price})

The Charting Library will call the callback every time the crosshair position is changed.

## Chart Actions

### setVisibleRange(range, callback)

1. `range`: object, `{from to}`
    * `from`, `to`: unix timestamps, UTC
1. `callback`: `function()`. The Library will call it after it's done with the viewport setup.

Forces the chart to adjust its parameters (scroll, scale) to make the selected time period fit the view port.
Neither `from`, nor `to` must not be in future. This method was introduced in version `1.2`.

### setSymbol(symbol, callback)

1. `symbol`: string
1. `callback`: function()

Makes the chart to change its symbol. Callback is called after new symbol's data arrived.

### setResolution(resolution, callback)

1. `resolution`: string. Format is described in another [article](Resolution.md).
1. `callback`: function()

Makes the chart to change its resolution. Callback is called after new data arrived.

### resetData()

Makes the chart to rerequest data from the data feed. Usually you need to call it when chart's data has changed.

Before calling this you should call [onResetCacheNeededCallback](JS-Api.md#subscribebarssymbolinfo-resolution-onrealtimecallback-subscriberuid-onresetcacheneededcallback).

### executeActionById(actionId)

*Since version 1.3.*

1. `actionId`: string

Executes an action by its id.

**Showing a dialog:**

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
* `lockDrawingsAction`
* `hideAllDrawingsAction`
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

*Since version 1.7.*

1. `actionId`: string

Get checkable action (e.g. `lockDrawingsAction`, `stayInDrawingModeAction`, `magnetAction`) state by its id (see ids of actions above)

### refreshMarks()

Calling this method makes the Library to request visible marks once again.

### clearMarks()

Calling this method makes the Library to remove all visible marks.

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

Calling this method closes a context menu or a dialog if it is shown.

### setTimezone(timezone)

1. `timezone`: string

See [timezone](Widget-Constructor.md#timezone) for more information.

Example:

```javascript
widget.activeChart().setTimezone('Asia/Singapore');
```

Makes the chart to change its timezone.

## Studies And Shapes

### getAllShapes()

Returns an array of all created shapes objects. Each object has following fields:

* `id`: id of a shape
* `name`: name of a shape

### getAllStudies()

Returns an array of all created shapes objects. Each object has following fields:

* `id`: id of a study
* `name`: name of a study

### setEntityVisibility(id, isVisible)

Sets visibility of an entity with passed id.

**Deprecated**: Use shape/study API instead (`getShapeById`/`getStudyById`). Will be removed in future releases.

### createStudy(name, forceOverlay, lock, inputs, callback, overrides, options)

1. `name`: string, a name of an indicator as you can see it in `Indicators` widget
1. `forceOverlay`: forces the Charting Library to place the created study on main pane
1. `lock`: boolean, shows whether a user will be able to remove/change/hide your study or not
1. `inputs`: (since version `1.2`) an array of study inputs. This array is expected to contain just inputs values in the same order they are printed in study's properties page.
1. `callback`: function(`entityId`)
1. `overrides`: (since version `1.2`) an object [containing properties](Studies-Overrides.md) you'd like to set for your new study. Note: you should not specify study name: start a property path with a plot name.
1. `options`: object with the only possible key `checkLimit`. If it is `true` study limit dialog will be shown if the limit if exceeded.

**Since 1.12 the function returns the result immediately. Callback is kept for compatibility.**

Creates a study on the main symbol. Examples:

* `createStudy('MACD', false, false, [14, 30, "close", 9])`
* `createStudy('Moving Average Exponential', false, false, [26])`
* `createStudy('Stochastic', false, false, [26], null, {"%d.color" : "#FF0000"})`
* `chart.createStudy('Moving Average', false, false, [26], null, {'Plot.linewidth': 10})`

**Remark**: `Compare` study has 2 inputs: `[dataSource, symbol]`. Supported `dataSource` values: `["close", "high", "low", "open"]`.

**Remark 2**: You actually use `Overlay` study when choose to `Add` a series on the chart. This study has the single input -- `symbol`. Here is an example how to add a symbol:

```javascript
    widget.chart().createStudy('Overlay', false, false, ['AAPL']);
```

**Remark 3**: You actually also use `Compare` study when choose to compare a series. This study has two inputs -- `source` and `symbol`. Here is an example how to add a compare series:

```javascript
    widget.chart().createStudy('Compare', false, false, ["open", 'AAPL']);
```

### getStudyById(entityId)

1. `entityId`: object. Value that is returned when a study is created via API.

Returns an object with the following methods to interact with a study:

1. `isUserEditEnabled()` - return `true` if a user is able to remove/change/hide your shape
1. `setUserEditEnabled(enabled)` - enables or disables removing/changing/hiding a study by a user
1. `getInputsInfo()` - returns an information about all inputs.

    Returned value is an array of objects with the following fields:
    * `id` - input id of the study
    * `name` - name of the input
    * `type` - type of the input
    * `localizedName` - name of the input translated to the current language

1. `getInputValues()` - returns values of study inputs.

    Returned value is an array of objects (`StudyInputValue`) with the following fields:
    * `id` - input id of the study
    * `value` - value of the input
1. `setInputValues(inputs)` - assigns input values to a study.
    `inputs` should be an array with objects of `StudyInputValue` (see above).
    It may contain only some of the inputs that you want to change.

1. `mergeUp()` - merges study up (if can)
1. `mergeDown()` - merges study down (if can)
1. `unmergeUp()` - unmerges study up (if can)
1. `unmergeDown()` - unmerges study down (if can)
1. `isVisible()` - returns `true` if the study is visible
1. `setVisible(visible)` - shows/hides the study
1. `bringToFront()` - raises the study on top of all other sources
1. `sendToBack()` - puts the study below all other sources
1. `applyOverrides(overrides)` - apply [overrides](https://github.com/tradingview/charting_library/wiki/Studies-Overrides) to the study.
    Keys of the `overrides` object don't need to start with the study name, since it is applied to the particular study.
    For example, you should use `style` instead of `Overlay.style` to override the current style of the Overlay study.

### createShape(point, options)

1. `point`: object `{time, [price], [channel]}`
    * `time`: unix time. The only mandatory argument.
    * `price`: If you specify `price`, then your icon will be placed on its level.
        If you do not, then the icon sticks to bar at respective time.
    * `channel`: The price level to stick to is specified by `channel` argument (`open`, `high`, `low`, `close`).
        If no channel is specified, 'open' is a default.
1. `options`: object `{shape, [text], [lock], [overrides]}`
    * `shape` may be one of the `arrow_up`, `arrow_down`, `flag`, `vertical_line`, `horizontal_line`.
        `flag` is the default value.
    * `text` is an optional argument. It's the text that will be assigned to shape if it can contain a text.
    * `lock` shows whether a user will be able to remove/change/hide your shape or not.
    * `disableSelection` (since `1.3`) prevents selecting of the shape
    * `disableSave` (since `1.3`) prevents saving the shape with a chart
    * `disableUndo` (since `1.4`) prevents adding of the action to the undo stack
    * `overrides` (since `1.2`). It is an object containing properties you'd like to set for your new shape.
    * `zOrder` (since `1.3`) may be one of the [`top`, `bottom`].
        `top` puts the line tool on top of all other sources, `bottom` puts the line tool below all other sources.
        If it is not specified the line tool is placed above all existing line tools.
    * `showInObjectsTree`: `true` by default. Displays the shape in the Objects Tree dialog.

The function returns `entityId` - unique id of the shape if creating is success or `null` otherwise.

This call creates a shape at specified point on main series.

### createMultipointShape(points, options)

1. `points`: an array of objects `[{time, [price], [channel]},...]`
    * `time`: unix time. The only mandatory argument.
    * `price`: If you specify `price`, then your icon will be placed on its level.
        If you do not, then the icon sticks to bar at respective time.
    * `channel`: The price level to stick to is specified by `channel` argument (`open`, `high`, `low`, `close`).
        If no channel is specified, 'open' is a default.
1. `options`: object `{shape, [text], [lock], [overrides]}`
    * `shape` may be one of the [identifiers](Shapes-and-Overrides.md)
    * `text` is an optional argument. It's the text that will be assigned to shape if it can contain a text.
    * `lock` shows whether a user will be able to remove/change/hide your shape or not.
    * `disableSelection` (since `1.3`) prevents selecting of the shape
    * `disableSave` (since `1.3`) prevents saving the shape with a chart
    * `disableUndo` (since `1.4`) prevents adding of the action to the undo stack
    * `overrides`. It is an object containing properties you'd like to set for your new shape.
    * `zOrder` (since `1.3`) may be one of the [`top`, `bottom`].
        `top` puts the line tool on top of all other sources, `bottom` puts the line tool below all other sources.
        If it is not specified the line tool is placed above all existing line tools.
    * `showInObjectsTree`: `true` by default. Displays the shape in the Objects Tree dialog.

The function returns `entityId` - unique id of the study if creating is success or `null` otherwise.

Look [Shapes and Overrides](Shapes-and-Overrides.md) for more information.

This call creates a shape with specified points on main series.

### getShapeById(entityId)

1. `entityId`: object. Value that is returned when a shape is created via API

Returns an object with the following methods to interact with a shape:

1. `isSelectionEnabled()` - returns `true` if the shape cannot be selected by a user
1. `setSelectionEnabled(enable)` - enables or disables selecting of the shape (see `disableSelection` option of `createMultipointShape`)
1. `isSavingEnabled()` - returns `true` if the shape is not saved to the chart
1. `setSavingEnabled(enable)` - enables or disables saving the shape into the chart layout (see `disableSave` option of `createMultipointShape`)
1. `isShowInObjectsTreeEnabled()` - returns `true` if the shape is displayed in the Objects Tree dialog
1. `setShowInObjectsTreeEnabled(enabled)` - enables or disables displaying of the shape in the Objects Tree dialog
1. `isUserEditEnabled()` - returns `true` if a user can remove/change/hide the shape
1. `setUserEditEnabled(enabled)` - enables or disables removing/changing/hiding of the shape by a user
1. `bringToFront()` - raises the line tool on top of all other sources
1. `sendToBack()` - puts the line tool below all other sources
1. `getProperties()` - get all properties of the line tool
1. `setProperties(properties)` - sets properties of the shape.
    `properties` should have the same structure as an object from `getProperties` but it can have only those properties that you want to override.
1. `getPoints()` - returns points of line tool. `Point` is an object `{ price, time }`.
1. `setPoints(points)` - set the new points to line tool.
    The point format is the same as `points` argument from `createMultipointShape` method.

### removeEntity(entityId)

1. `entityId`: object. Value which was returned when the entity (shape of study) was created.

Removes the specified entity.

### removeAllShapes()

Removes all shapes (drawings) from the chart.

### removeAllStudies()

Removed all studies from the chart.

## Study Templates

### createStudyTemplate(options)

1. `options`: object `{saveInterval}`
    * `saveInterval`: boolean

Saves the study template to JS object. Charting Library will call your callback and pass the state object as argument.

This call is a part of low-level [save/load API](Saving-and-Loading-Charts.md).

### applyStudyTemplate(template)

1. `template`: object

Loads the study template from state object.

This call is a part of low-level [save/load API](Saving-and-Loading-Charts.md).

## Trading Primitives

### createOrderLine(options)

1. `options` *(since version 1.4)* is an object with one possible key: `disableUndo` which can be `true` or `false`.
    For compatibility reasons the default value is `false`.

Creates a new order on the chart and returns an API-object which you can use to control the order properties and behavior.

It's strongly recommended to read [this article](Trading-Primitives.md) before using this call.

API object methods:

* `remove()`: Removes the position from the chart. You can’t use API-object after this call.
* `onModify(callback)` / `onModify(data, callback)`
* `onMove(callback)` / `onMove(data, callback)`

API object has a set of properties listed below. Each property should be used through respective accessors.
I.e., if you want to work with `Extend Left` property, use `getExtendLeft()` of `setExtendLeft()` methods.

**General properties**:

Property|Type|Supported Values|Default Value
---|---|---|---
Price|Double|Double|0.0
Text|String|String|""
Tooltip|String|String|""
Quantity|String|String|""
Editable|Boolean|Boolean|true

**Connection line properties**:

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

1. `options` *(since version 1.4)* is an object with one possible key: `disableUndo` which can be `true` or `false`.
    For compatibility reasons the default value is `false`.

Creates a new position on the chart and returns an API-object which you can use to control the position properties and behavior.

It's strongly recommended to read [this article](Trading-Primitives.md) before using this call.

API object methods:

* `remove()`: Removes the position from the chart. You can’t use API-object after this call.
* `onClose(callback)` / `onClose(data, callback)`
* `onModify(callback)` / `onModify(data, callback)`
* `onReverse(callback)` / `onReverse(data, callback)`

API object has a set of properties listed below. Each property should be used through respective accessors.
I.e., if you want to work with `Extend Left` property, use `getExtendLeft()` of `setExtendLeft()` methods.

**General properties**:

Property|Type|Supported Values|Default Value
---|---|---|---
Price|Double|Double|0.0
Text|String|String|""
Tooltip|String|String|""
Quantity|String|String|""

**Connection line properties**:

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

1. `options` *(since version 1.4)* is an object with one possible key: `disableUndo` which can be `true` or `false`.
    For compatibility reasons the default value is `false`.

Creates a new execution on the chart and returns an API-object which you can use to control the execution properties.

It's strongly recommended to read [this article](Trading-Primitives.md) before using this call.

API object has a set of properties listed below. Each property should be used through respective accessors.
I.e., if you want to work with `Extend Left` property, use `getExtendLeft()` of `setExtendLeft()` methods.

API object methods:

* `remove()`: Removes the execution shape from the chart. You can’t use API-object after this call.

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

Returns chart's symbol.

### symbolExt()

Returns chart's symbol information object. The object has the following fields:

* `symbol`: the same as [symbol()](#symbol) method result
* `full_name`: full symbol name
* `exchange`: symbol's exchange
* `description`: symbol's description
* `type`: symbol's type

### resolution()

Returns chart's resolution. Format is described in another [article](Resolution.md).

### getVisibleRange()

Returns object `{from, to}`. `from` and `to` are Unit timestamps **in the timezone of chart**.

### getVisiblePriceRange()

*Since version 1.7.*

Returns object `{from, to}`. `from` and `to` are boundaries of main series price scale visible range.

### priceFormatter()

Returns object with `format` function that you can use to format prices. Introduced in 1.5.

### chartType()

Returns the main series style.

## See Also

* [Widget Methods](Widget-Methods.md)
* [Customization Overview](Customization-Overview.md)
* [Widget Constructor](Widget-Constructor.md)
* [Saving and Loading Charts](Saving-and-Loading-Charts.md)
* [Overriding Studies' Defaults](Studies-Overrides.md)
* [Overriding Chart's Defaults](Overrides.md)
