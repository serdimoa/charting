Here is a list of supported widget's methods. You can call them using widget object returned to you by widget's constructor.

**Remark**: Please note that it's safe to call any method only **after** onChartReady callback is fired. So the common practice is to do smth like

```javascript
widget.onChartReady(function() {
    // now it's safe to call any other widget's methods
});
```

#Methods

**Before 1.5 [[Chart Methods]] belonged to the Widget. Please see the full list of actions [[here|Chart-Methods]]**

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
  * [[subscribe(event, callback)|Widget-Methods#subscribeevent-callback]]
* Chart Actions
  * [[chart()|Widget-Methods#chart]]
  * [[setLanguage(locale)|Widget-Methods#setlanguagelocale]]
  * [[setSymbol(symbol, interval, callback)|Widget-Methods#setsymbolsymbol-interval-callback]]
  * [[remove()|Widget-Methods#remove]]
  * [[closePopupsAndDialogs()|Widget-Methods#closepopupsanddialogs]]
* Saving/Loading Charts
  * [[save(callback)|Widget-Methods#savecallback]]
  * [[load(state)|Widget-Methods#loadstate]]
* Custom UI Controls
  * [[onContextMenu(callback)|Widget-Methods#oncontextmenucallback]]
  * [[createButton(options)|Widget-Methods#createbuttonoptions]]
* Getters
  * [[symbolInterval(callback)|Widget-Methods#symbolintervalcallback]]
  * [[mainSeriesPriceFormatter()|Widget-Methods#mainseriespriceformatter]]
* Customization
  * [[addCustomCSSFile(url)|Widget-Methods#addcustomcssfileurl]]
  * [[applyOverrides(overrides)|Widget-Methods#applyoverridesoverrides]]
* :chart: [[Trading Platform]] specific 
  * [[isFloatingTradingPanelVisible()|Widget-Methods#chart-isfloatingtradingpanelvisible]]
  * [[toggleFloatingTradingPanel()|Widget-Methods#chart-togglefloatingtradingpanel]]
  * [[isBottomTradingPanelVisible()|Widget-Methods#chart-isbottomtradingpanelvisible]]
  * [[toggleBottomTradingPanel()|Widget-Methods#chart-togglebottomtradingpanel]]
  * [[showSampleOrderDialog(order)|Widget-Methods#chart-showsampleorderdialogorder]]
  * [[showSamplePositionDialog(position)|Widget-Methods#chart-showsamplepositiondialogposition]]
  * [[showSampleClosePositionDialog(position)|Widget-Methods#chart-showsampleclosepositiondialogposition]]
  * [[showSampleReversePositionDialog(position)|Widget-Methods#chart-showsamplereversepositiondialogposition]]
* :chart: Multiple Charts Layout
  * [[chart(index)|Widget-Methods#chart-chartindex]]
  * [[activeChart()|Widget-Methods#chart-activechart]]
  * [[chartsCount()|Widget-Methods#chart-chartscount]]
  * [[layout()|Widget-Methods#chart-layout]]
  * [[setLayout(layout)|Widget-Methods#chart-setlayoutlayout]]

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

####subscribe(event, callback)
1. `event`: can be 
  * `toggle_sidebar` - drawing toolbar is shown/hidden
  * `indicators_dialog` - Indicators dialog is shown
  * `toggle_header` - chart header is shown/hidden
  * `edit_object_dialog` - Chart/Study Properties dialog is shown
  * `chart_load_requested` - new chart about to be loaded
  * `chart_loaded`
  * :chart: `layout_about_to_be_changed` - amount or placement of charts about to be changed
  * :chart: `layout_changed` - amount or placement of charts is changed
  * :chart: `activeChartChanged` - active chart is changed
2. `callback`: function(arguments)

The library will call `callback` when GUI `event` is happened. Every event can have different set of arguments.

#Chart Actions

####chart()

Returns a chart object that you can use to call [[Chart-Methods]]

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

####closePopupsAndDialogs()

Calling this method closes a context menu or a dialog if it is shown.

#Saving/Loading Charts


####save(callback)
1. `callback`: function(object)

Saves the chart state to JS object. Charting Library will call your callback and pass the state object as argument. This call is a part of low-level [[save/load API|Saving-and-Loading-Charts]].

####load(state)
1. `state`: object

Loads the chart from state object. This call is a part of low-level [[save/load API|Saving-and-Loading-Charts]].

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

#Getters

####symbolInterval(callback)
1. `callback`: function(result)
  1. `result`: object `{symbol, interval}`

**Since 1.4 the function returns the result immediately. Callback is kept for compatability.**

Charting Library will call your callback with an object containing chart's symbol and interval.

####mainSeriesPriceFormatter()

Returns object with method `format` that you can use to format prices. Introduced in 1.5.


#Customization


####addCustomCSSFile(url)
1. `url` should be absolute or relative path to 'static` folder

This method was introduced in version `1.3`. Starting from `1.4` use [custom_css_url](https://github.com/tradingview/charting_library/wiki/Widget-Constructor#custom_css_url) instead.

####applyOverrides(overrides)
*Introduced in Charting Library 1.5*

1. `overrides` is an object. It is the same as [overrides](https://github.com/tradingview/charting_library/wiki/Widget-Constructor#overrides) in Widget Constructor.

This method applies overrides to properties without reloading the chart.

#:chart: Trading Platform

The following methods are available in [[Trading Platform]] only.

####:chart: isFloatingTradingPanelVisible()

This method returns `true` if the Floating Trading Panel is visible and `false` otherwise.

####:chart: toggleFloatingTradingPanel()

This method hides the Floating Trading Panel if it is visible and shows otherwise.

####:chart: isBottomTradingPanelVisible()

This method returns `true` if the Bottom Trading Panel is visible and `false` otherwise.

####:chart: toggleBottomTradingPanel()

This method hides the Bottom Trading Panel if it is visible and shows otherwise.

####:chart: showSampleOrderDialog(order)
####:chart: showSamplePositionDialog(position)
####:chart: showSampleClosePositionDialog(position)
####:chart: showSampleReversePositionDialog(position)
1. `order` or 'position': object 

Displays a sample order/position dialog. These dialogs look like Trading View Paper Trading ones. Usually you don't need to use sample dialogs. These methods are used in the trading sample.

#:chart: Multiple Charts Layout

####:chart: chart(index)
1. `index`: index of a chart starting from 0. `index` is 0 by default.

Returns a chart object that you can use to call [[Chart-Methods]]

####:chart: activeChart()

Returns current active chart object that you can use to call [[Chart-Methods]]

####:chart: chartsCount()

Returns amount of charts in the current layout

####:chart: layout()

Returns current layout mode. Possible values: `4`, `6`, `8`, `s`, `2h`, `2-1`, `2v`, `3h`, `3v`, `3s`.

####:chart: setLayout(layout)
1. `layout`: Possible values: `4`, `6`, `8`, `s`, `2h`, `2-1`, `2v`, `3h`, `3v`, `3s`.

Changes current chart layout.

#See Also
* [[Chart-Methods]]
* [[Charts Customization 101]]
* [[Widget Constructor]]
* [[Saving and Loading Charts|Saving-and-Loading-Charts]]
* [[Overriding Studies' Defaults|Studies-Overrides]]
* [[Overriding Chart's Defaults|Overrides]]