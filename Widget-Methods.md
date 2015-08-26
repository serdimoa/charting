Here is a list of supported widget's methods. You can call them using widget object returned to you by widget's constructor.

**Remark**: Please note that it's safe to call any method only **after** onChartReady callback is fired. So the common practice is to do smth like

```javascript
widget.onChartReady(function() {
    // now it's safe to call any other widget's methods
});
```

#Methods

* Subscribing To Chart Events
  * [[onChartReady(callback)|Widget-Methods#onchartreadycallback]]
  * [[onSymbolChange(callback)|Widget-Methods#onsymbolchangecallback]]
  * [[onIntervalChange(callback)|Widget-Methods#onintervalchangecallback]]
  * [[onAutoSaveNeeded(callback)|Widget-Methods#onautosaveneededcallback]]
  * [[onBarMarkClicked(callback)|Widget-Methods#onbarmarkclickedcallback]]
  * [[onTimescaleMarkClicked(callback)|Widget-Methods#ontimescalemarkclickedcallback]]
  * [[onGrayedObjectClicked(callback)|Widget-Methods#ongrayedobjectclickedcallback]]
  * [[onScreenshotReady(callback)|Widget-Methods#onscreenshotreadycallback]]
  * [[onTick(callback)|Widget-Methods#ontickcallback]]
  * [[onShortcut(shortcut, callback)|Widget-Methods#onshortcutshortcut-callback]]
* Chart Actions
  * [[setVisibleRange(range, callback)|Widget-Methods#setvisiblerangerange-callback]]
  * [[setLanguage(locale)|Widget-Methods#setlanguagelocale]]
  * [[setSymbol(symbol, interval, callback)|Widget-Methods#setsymbolsymbol-interval-callback]]
  * [[remove()|Widget-Methods#remove]]
  * [[executeAction(action)|Widget-Methods#executeactionaction]]
  * [[executeActionById(action)|Widget-Methods#executeactionbyidactionid]]
  * [[refreshMarks()|Widget-Methods#refreshmarks]]
  * [[clearMarks()|Widget-Methods#clearmarks]]
  * [[setChartType(type)|Widget-Methods#setcharttypetype]]
  * [[closePopupsAndDialogs()|Widget-Methods#closepopupsanddialogs]]
* Studies And Shapes 
  * [[createStudy(name, forceOverlay, lock, inputs, callback, overrides)|Widget-Methods#createstudyname-forceoverlay-lock-inputs-callback-overrides]]
  * [[createShape(point, options, callback)|Widget-Methods#createshapepoint-options-callback]]
  * [[createMultipointShape(points, options, callback)|Widget-Methods#createmultipointshapepoints-options-callback]]
  * [[removeEntity(entityId)|Widget-Methods#removeentityentityid]]
  * [[createVerticalLine(point, options)|Widget-Methods#createverticallinepoint-options]]
  * [[removeAllShapes()|Widget-Methods#removeallshapes]]
  * [[removeAllStudies()|Widget-Methods#removeallstudies]]
* Saving/Loading Charts
  * [[save(callback)|Widget-Methods#savecallback]]
  * [[load(state)|Widget-Methods#loadstate]]
* Study Templates
  * [[createStudyTemplate(options, callback)|Widget-Methods#createstudytemplateoptions-callback]]
  * [[applyStudyTemplate(template)|Widget-Methods#applystudytemplatetemplate]]
* Custom UI Controls
  * [[onContextMenu(callback)|Widget-Methods#oncontextmenucallback]]
  * [[createButton(options)|Widget-Methods#createbuttonoptions]]
* Trading Primitives
  * [[createOrderLine()|Widget-Methods#createorderline]]
  * [[createPositionLine()|Widget-Methods#createpositionline]]
  * [[createExecutionShape()|Widget-Methods#createexecutionshape]]
* Getters
  * [[symbolInterval(callback)|Widget-Methods#symbolintervalcallback]]
  * [[getVisibleRange(callback)|Widget-Methods#getvisiblerangecallback]]
* Customization
  * [[addCustomCSSFile(url)|Widget-Methods#addcustomcssfileurl]]

#Subscribing To Chart Events

####onChartReady(callback)
1. `callback`: function()

The Charting Library will call the callback provided once when chart is initialized and ready. You can safely call all other methods from this moment.

####onSymbolChange(callback)
1. `callback`: function(symbolData)
  1.  `symbolData`: object `{name, exchange, description, type, interval}`

The Charting Library will call the callback provided every time the main series symbol changes. New symbol info will be passed as argument.

####onIntervalChange(callback)
1. `callback`: function(interval)
  1. `interval`: string

The Charting Library will call the callback provided every time the main series interval changes. New interval will be passed as argument.

####onAutoSaveNeeded(callback)
1. `callback`: function()

The Library will call the callback provided every time when user changes the chart. `Chart change` means any user action that can be undone. The callback will not be called more than once in five seconds.

####onBarMarkClicked(callback)
1. `callback`: function(markId)

The Library will call the callback provided every time when user clicks a [[mark on bar|Marks-On-Bars]]. Mark ID will be passed as an argument.

####onTimescaleMarkClicked(callback)
1. `callback`: function(markId)

The Library will call the callback provided every time when user clicks a timescale mark. Mark ID will be passed as an argument.


####onGrayedObjectClicked(callback)
1. `callback`: function(subject)
  1. `subject`: object `{type, name}`
    1. `type`: `drawing` | `study`
    2. `name`: string, name of a subject which was clicked

The Library will call the callback provided every time when user clicks on grayed out object. Example:

```javascript
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
        // this function will be called when one tries to
        // create Balance Of Power study or Trend Angle shape

        alert(data.name + " is grayed out!");
    })
});

```

####onScreenshotReady(callback)
1. `callback`: function(imageName)

The Library will call the callback provided every time when user creates a screenshot and server returns the created image name.

####onTick(callback)
1. `callback`: function(data)

The Library will call the callback provided every time when recent bar updates.

####onShortcut(shortcut, callback)
1. `shortcut`
2. `callback`: function(data)

The Library will call the callback provided every time when shortcut is pressed.

Example:

```javascript
widget.onShortcut("alt+s", function() {
  widget.executeActionById("symbolSearch");
});
```

#Chart Actions

####setVisibleRange(range, callback)
1. `range`: object, `{from to}`
  1. `from`, `to`: unix timestamps, UTC
2. `callback`: `function()`. The Library will call it after it's done with the viewport setup.

Forces the chart to adjust its parameters (scroll, scale) to make the selected time period fit the view port.
Neither `from`, nor `to` must not be in future. This method was introduced in version `1.2`.

####setLanguage(locale)
1. `locale`: [[language code|Localization]]

Sets the Widget's language. For now, this call reloads the chart. **Please avoid using it**.

####setSymbol(symbol, interval, callback)
1. `symbol`: string
2. `interval`: string
3. `callback`: function()

Makes the chart to change its symbol and resolution. Callback is called after new symbol's data arrived.

####remove()
Removes chart widget from your page.

####executeAction(action)
**_deprecated, use executeActionById instead_**

1. `action`: string

Executes any action from chart's context menu (the menu which is popped up when one right-clicks the empty space on a main pane) by its name. Use names as you see them in English localization. Examples:
```javascript
// < ... >
widget.executeAction("Insert Indicator..."); // calling this will show `Insert Study` dialog
// < ... >
widget.executeAction("Hide All Drawing Tools"); // this will toggle all shapes visibility
// < ... >
```

####executeActionById(actionId)
_**since version 1.3**_

1. `actionId`: string

Executes an action by its id.

**Showing a dialog**

	chartProperties 
	compareOrAdd
	scalesProperties
	tmzProperties 
	paneObjectTree
	insertIndicator 
	symbolSearch
	changeInterval

**Other actions**

	timeScaleReset
	chartReset
	seriesHide
	studyHide 
	lineToggleLock
	lineHide 
	showLeftAxis
	showRightAxis
	scaleSeriesOnly
	drawingToolbarAction
	magnetAction
	stayInDrawingModeAction
	lockDrawingsAction
	hideAllDrawingsAction 
	hideAllMarks 
	showCountdown 
	showSeriesLastValue
	showSymbolLabelsAction
	showStudyLastValue
	showStudyPlotNamesAction
	undo
	redo
	takeScreenshot

Examples:
```javascript
// < ... >
widget.executeActionById("undo");
// < ... >
widget.executeActionById("drawingToolbarAction"); // hides or shows the drawing toolbar
// < ... >
```

####refreshMarks()

Calling this method makes the Library to request visible marks once again.

####clearMarks()

Calling this method makes the Library to remove all visible marks.

####setChartType(type)
1. `type`: `TradingView.BARS` | `TradingView.CANDLES` | `TradingView.AREA` | `TradingView.LINE` | `TradingView.HEIKEN_ASHI` | `TradingView.HOLLOW_CANDLES` 

Sets the main series style.

####closePopupsAndDialogs()

Calling this method closes a context menu or a dialog if it is shown.

#Studies And Shapes

####createStudy(name, forceOverlay, lock, inputs, callback, overrides)
1. `name`: string, a name of an indicator as you can see it in `Indicators` widget
2. `forceOverlay`: forces the Charting Library to place the created study on main pane
3. `lock`: boolean, shows whether a user will be able to remove/change/hide your study or not
4. `inputs`: (since version `1.2`) an array of study inputs. This array is expected to contain just inputs values in the same order they are printed in study's properties page.
5. `callback`: function(`entityId`)
6. `overrides`: (since version `1.2`) an object [containing properties](https://github.com/tradingview/charting_library/wiki/Studies-Overrides) you'd like to set for your new study.

Creates the study on a main symbol. Examples: 
  * `createStudy('MACD', false, false, [14, 30, 9])`
  * `createStudy('Moving Average Exponential', false, false, [26])`
  * `createStudy('Stochastic', false, false, [26], null, {"%d.color" : "#FF0000"})`

**Remark**: `Compare` study has 2 inputs: `[dataSource, symbol]`. Supported `dataSource` values: `["close", "high", "low", "open"]`.

**Remark 2**: You actually use `Overlay` study when choose to `Add` a series on the chart. This study has the single input -- `symbol`. Here is an example how to add a symbol:

```javascript
    widget.createStudy('Overlay', false, false, ['AAPL']);
```

####createShape(point, options, callback)
1. `point`: object `{time, [price], [channel]}`
  1. `time`: unix time. The only mandatory argument.
  2. `price`: If you specify `price`, then your icon will be placed on its level. If you do not, then the icon sticks to bar at respective time.
  3. `channel`: The price level to stick to is specified by `channel` argument (`open`, `high`, `low`, `close`). If no channel is specified, 'open' is a default.
2. `options`: object `{shape, [text], [lock], [overrides]}`
  1. `shape` may be one of the ['arrow_up', 'arrow_down', 'flag', 'vertical_line', 'horizontal_line']. 'flag' is the default value.
  2. `text` is an optional argument. It's the text that will be assigned to shape if it can contain a text.
  3. `lock` shows whether a user will be able to remove/change/hide your shape or not.
  4. `disableSelection` (since `1.3`) prevents selecting of the shape
  5. `disableSave` (since `1.3`) prevents saving the shape with a chart
  6. `overrides` (since `1.2`). It is an object containing properties you'd like to set for your new shape.
  7. `zOrder` (since `1.3`) may be one of the [`top`, `bottom`]. `top` puts the line tool on top of all other sources, `bottom` puts the line tool below all other sources. If it is not specified the line tool is placed above all existing line tools.
3. `callback`: function(`entityId`)

This call creates a shape at specified point on main series. 

####createMultipointShape(points, options, callback)
1. `points`: an array of objects `[{time, [price], [channel]},...]`
  1. `time`: unix time. The only mandatory argument.
  2. `price`: If you specify `price`, then your icon will be placed on its level. If you do not, then the icon sticks to bar at respective time.
  3. `channel`: The price level to stick to is specified by `channel` argument (`open`, `high`, `low`, `close`). If no channel is specified, 'open' is a default.
2. `options`: object `{shape, [text], [lock], [overrides]}`
  1. `shape` may be one of the [[identifiers|Shapes and Overrides]]
  2. `text` is an optional argument. It's the text that will be assigned to shape if it can contain a text.
  3. `lock` shows whether a user will be able to remove/change/hide your shape or not.
  4. `disableSelection` (since `1.3`) prevents selecting of the shape
  5. `disableSave` (since `1.3`) prevents saving the shape with a chart
  6. `overrides`. It is an object containing properties you'd like to set for your new shape.
  7. `zOrder` (since `1.3`) may be one of the [`top`, `bottom`]. `top` puts the line tool on top of all other sources, `bottom` puts the line tool below all other sources. If it is not specified the line tool is placed above all existing line tools.
3. `callback`: function(`entityId`)

Look [[Shapes and Overrides|Shapes and Overrides]] for more information.

This call creates a shape with specified points on main series.

####removeEntity(entityId)
1. `entityId`: object. Value which was passed to your callback after the entity (shape of study) was created.

Removes the specified entity.

####createVerticalLine(point, options)
1. `point`: object `{time}`
2. `options`: obejct `{lock}`

This function is a synonym for `createShape` with shape = 'vertical_line'. It is treated as **obsolete**.

####removeAllShapes()
Removes all shapes (drawings) from the chart.

####removeAllStudies()
Removed all studies from the chart.


#Saving/Loading Charts


####save(callback)
1. `callback`: function(object)

Saves the chart state to JS object. Charting Library will call your callback and pass the state object as argument. This call is a part of low-level [[save/load API|Saving-and-Loading-Charts]].

####load(state)
1. `state`: object

Loads the chart from state object. This call is a part of low-level [[save/load API|Saving-and-Loading-Charts]].

#Study Templates


####createStudyTemplate(options, callback)
1. `options`: object `{saveInterval}`
 1. `saveInterval`: boolean
2. `callback`: function(data)

Saves the study template to JS object. Charting Library will call your callback and pass the state object as argument. This call is a part of low-level [[save/load API|Saving-and-Loading-Charts]].

####applyStudyTemplate(template)
1. `template`: object 

Loads the study template from state object. This call is a part of low-level [[save/load API|Saving-and-Loading-Charts]].


#Custom UI Controls

####onContextMenu(callback)
1. `callback`: function(unixtime, price). This callback is expected to return a value (see below).

The Library will call the callback provided every time when user opens context menu on the chart. Unix time and price of context menu point will be provided as arguments. To customize context menu items you have to return array of items descriptors. Item descriptor has following structure:
```
{
    position: 'top' | 'bottom',
    text: 'Menu item text',
    click: <onItemClicked callback>
}
```
* `position`: position of item in context menu
* `text`: text of menu item
* `click`: callback which will be called when user select your menu item

To add a separator use minus sign. Example: `{ text: "-", position: "top" }`.

To remove an existing item from a menu use minus sign in front of the item text. 
Example: `{ text: "-Objects Tree..." }`

Example: 
```javascript
widget.onChartReady(function() {
    widget.onContextMenu(function(unixtime, price) {
        return [{
            position: "top",
            text: "First top menu item, time: " + unixtime + ", price: " + price,
            click: function() { alert("First clicked."); }
        }, 
        { text: "-", position: "top" },
        { text: "-Objects Tree..." },
        {
            position: "top",
            text: "Second top menu item 2",
            click: function() { alert("Second clicked."); }
        }, {
            position: "bottom",
            text: "Bottom menu item",
            click: function() { alert("Third clicked."); }
        }];
    });
```

####createButton(options)
1. `options`: object `{ align: "left" }`
  1. `align`: "right" | "left". default: "left"

Creates a new DOM element in chart top toolbar and returns **jQuery object** for this button. You can use it to append custom controls right on the chart. Example:

```javascript
widget.onChartReady(function() {
    widget.createButton()
        .attr('title', "My custom button tooltip")
        .on('click', function (e) { alert("My custom button pressed!"); })
        .append($('<span>My custom button caption</span>'));
});
```

#Trading Primitives


####createOrderLine()
Creates a new order on the chart and returns an API-object which you can use to control the order properties and behavior. It's strongly recommended to read [[this article|Trading-Primitives]] before using this call.

API object methods:
* `remove()`: Removes the position from the chart. You can’t use API-object after this call.
* `onModify(callback)` / `onModify(data, callback)`
* `onMove(callback)` / `onMove(data, callback)`

API object has a set of properties listed below. Each property should be used through respective accessors. I.e., if you want to work with `Extend Left` property, use `getExtendLeft()` of `setExtendLeft()` methods.

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
widget.createOrderLine()
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

####createPositionLine()
Creates a new position on the chart and returns an API-object which you can use to control the position properties and behavior.  It's strongly recommended to read [[this article|Trading-Primitives]] before using this call.

API object methods:
* `remove()`: Removes the position from the chart. You can’t use API-object after this call.
* `onClose(callback)` / `onClose(data, callback)`
* `onModify(callback)` / `onModify(data, callback)`
* `onReverse(callback)` / `onReverse(data, callback)`

API object has a set of properties listed below. Each property should be used through respective accessors. I.e., if you want to work with `Extend Left` property, use `getExtendLeft()` of `setExtendLeft()` methods.

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
widget.createPositionLine()
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
####createExecutionShape()
Creates a new execution on the chart and returns an API-object which you can use to control the execution properties. It's strongly recommended to read [[this article|Trading-Primitives]] before using this call.

API object has a set of properties listed below. Each property should be used through respective accessors. I.e., if you want to work with `Extend Left` property, use `getExtendLeft()` of `setExtendLeft()` methods.

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
widget.createExecutionShape()
    .setText("@1,320.75 Limit Buy 1")
    .setTooltip("@1,320.75 Limit Buy 1")
    .setTextColor("rgba(0,255,0,0.5)")
    .setArrowColor("#0F0")
    .setDirection("buy")
    .setTime(1413559061758)
    .setPrice(15.5);
```

#Getters

####symbolInterval(callback)
1. `callback`: function(result)
  1. `result`: object `{symbol, interval}`

Charting Library will call your callback with an object containing chart's symbol and interval.

####getVisibleRange(callback)
1. `callback`: function(range)
  1. `range`: object `{from, to}`. `from` and `to` are Unit timestamps **in the timezone of chart**.

The Library will call your callback and pass the current visible time range. This method was introduced in version `1.2`.

#Customization


####addCustomCSSFile(url)
1. `url`should be absolute or relative path to 'static` folder

This method was introduced in version `1.3`.

#See Also
* [[Charts Customization 101]]
* [[Widget Constructor]]
* [[Saving and Loading Charts|Saving-and-Loading-Charts]]
* [[Overriding Studies' Defaults|Studies-Overrides]]
* [[Overriding Chart's Defaults|Overrides]]