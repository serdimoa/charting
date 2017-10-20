We do our best to make every next version fully compartible with the previous one, but sometimes big changes requires you to make minor changes in your code when upgrading to a new version.

_Note: you can check Charting Library version by executing `TradingView.version()` in a browser console._

Here is the list of breaking changes:

**Version 1.12**

**Charting Library**

- `charting_library/charting_library.min.js` now is [UMD](https://github.com/umdjs/umd) module.
If you just inline this script into HTML - nothing changed.
But if import it as a module you should import `widget`, `version` and `onready` functions straight from it.

Study overrides:
- Overrides for `Overlay` should be applied only via `studies_overrides` (and `applyStudiesOverrides` in runtime). In previous versions you had to use `overrides` and `applyOverrides`). See [[Studies-Overrides|Studies-Overrides]] page.
- Since this version you cannot override `showStudyArguments` and `showLastValue` using `options` keyword.

**Trading Terminal**

- `hasHistory` flag is removed. Use `historyColumns` to display History in Account Manager.

The following stuff is still supported in Trading Terminal, but will be deprecated in future versions:
- `subscribePL` and `unsubscribePL`. The broker should call `plUpdate` method of the Host every time when the profit is changed.
- `supportDOM` removed. DOM widget visibility can be set using `dome_widget` featureset.

**The Trading Controller is replaced with [[Broker API]]**.

The following changes should be applied to your Trading Controller implementation to move to new Broker API:
- Method `setHost` is removed. Host should be passed to the constructor of Broker API.
- Method `buttonDropdownItems` removed. Broker API should update [[Broker API|Trading-Host]] using `setButtonDropdownActions`.
- Methods `configFlags` and `durations` are removed. Use [[Widget Constructor]] fields instead.
- All methods that returned `$.Deferred` should be modified to return Promise.
- Method `chartContextMenuItems` is renamed to `chartContextMenuActions`.
- Method `isTradable` changed to return a Promise instead of a Boolean value.
- All string constants ("working", "buy" etc.) should be replaced with the appropriate number constants.
- Position `avg_price` renamed to `avgPrice`.
- `tradingController` field in the [[Widget Constructor]] is removed. Use `brokerFactory` instead.

**Version 1.11**

**Trading Terminal**

The following stuff is still supported in Trading Terminal, but will be deprecated in future versions:
- `supportDOME` renamed to `supportDOM`.
- Changed signature of `showClosePositionDialog.
- `showEditBracketsDialog` renamed to `showPositionBracketsDialog`, changed signature.

**Version 1.10**
- Default behavior of Volume indicator is changed.

Previous behavior: Volume indicator is added/removed when an instrument or a resolution is switched depending on volume support by the instrument. You can get back to this behavior by disabling `create_volume_indicator_by_default_once` featureset.

New behavior: Volume indicator is added on first load of an empty chart if it is supported by an active instrument.

**Version 1.9**
- We don't compile Pine scripts anymore. You can still use scripts compiled before.

**Version 1.8 of Trading Terminal**
-  Chart has no option to show only actual orders anymore. Appropriate methods have been removed.
- `showOrderDialog` receives an object instead of arguments list
- `showSampleOrderDialog` removed, use [[showOrderDialog|Trading-Host#showorderdialogorder-handler]] instead
- `showOrderDialog` removed from [[Broker API|Broker API]], use `placeOrder` and `modifyOrder` receive `silently` argument instead
- `reversePosition`, `closePosition`, `cancelOrder` have an additional argument `silently`. They should should appropriate dialogs by themselves from now.

**Version 1.7**

- Since this version it is not enough to call `setSymbol` with the same symbol. You should call `onResetCacheNeededCallback` from `subscribeBars` first. Then you can use `setSymbol` or new `resetData` method of the chart.
- JSAPI protocol version 1 is not supported any more. `nextTime` and `noData` must be provided.

**Version 1.5**

* Added `source` argument to MACD. You should change MACD creation code to pass `source` also.
`chartWidget.chart().createStudy('MACD', false, false, [12, 26, "close", 9])`

**Version 1.4**

* Override `transparency` is not supported anymore. We added transparency support to every color property. Use `rgba` form to define a color with transparency. Example: 
`"symbolWatermarkProperties.color" : "rgba(60, 70, 80, 0.05)"`

**Version 1.3**

* Override `paneProperties.gridProperties.*` is not supported anymore. 
Please use `paneProperties.vertGridProperties.*` and `paneProperties.horzGridProperties.*`

* Override `mainSeriesProperties.candleStyle.wickColor` is not supported anymore.
Use `mainSeriesProperties.candleStyle.wickUpColor` and `mainSeriesProperties.candleStyle.wickDownColor`
