We do our best to make every next version fully compartible with the previous one, but sometimes big changes requires you to make minor changes in your code when upgrading to a new version.

_Note: you can check Charting Library version by executing `TradingView.version()` in a browser console._

Here is the list of breaking changes:

**In version 1.4**

* Override `transparency` is not supported anymore. We added transparency support to every color property. Use `rgba` form to define a color with transparency. Example: 
`"symbolWatermarkProperties.color" : "rgba(60, 70, 80, 0.05)"`

**In version 1.3**

* Override `paneProperties.gridProperties.*` is not supported anymore. 
Please use `paneProperties.vertGridProperties.*` and `paneProperties.horzGridProperties.*`

* Override `mainSeriesProperties.candleStyle.wickColor` is not supported anymore.
Use `mainSeriesProperties.candleStyle.wickUpColor` and `mainSeriesProperties.candleStyle.wickDownColor`