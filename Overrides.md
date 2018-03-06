This Charting Library section includes the chart property description. These properties are treated
as customizable ones. The customizaion of other properties is not supported.

The properties are listed in the following format:

`<property_path>: <default Charting Library value>`

```javascript
// supported values: large, medium, small, tiny
volumePaneSize: "large"

// fonts available in text editors (i.e., in `Text` drawing tool properties dialog)
editorFontsList: ['Verdana', 'Courier New', 'Times New Roman', 'Arial']

paneProperties.background: "#ffffff"
paneProperties.vertGridProperties.color: "#E6E6E6"
paneProperties.vertGridProperties.style: LINESTYLE_SOLID
paneProperties.horzGridProperties.color: "#E6E6E6"
paneProperties.horzGridProperties.style: LINESTYLE_SOLID
paneProperties.crossHairProperties.color: "#989898"
paneProperties.crossHairProperties.width: 1
paneProperties.crossHairProperties.style: LINESTYLE_DASHED

// Margins (percentage). Used for auto scaling.
paneProperties.topMargin: 5
paneProperties.bottomMargin: 5

// leftAxisProperties & rightAxisProperties
paneProperties.leftAxisProperties.autoScale:true                    (see #749)
paneProperties.leftAxisProperties.autoScaleDisabled:false           (see #749)
paneProperties.leftAxisProperties.percentage:false
paneProperties.leftAxisProperties.percentageDisabled:false
paneProperties.leftAxisProperties.log:false
paneProperties.leftAxisProperties.logDisabled:false
paneProperties.leftAxisProperties.alignLabels:true

paneProperties.legendProperties.showStudyArguments: true
paneProperties.legendProperties.showStudyTitles: true
paneProperties.legendProperties.showStudyValues: true
paneProperties.legendProperties.showSeriesTitle: true
paneProperties.legendProperties.showSeriesOHLC: true

scalesProperties.showLeftScale : false
scalesProperties.showRightScale : true
scalesProperties.backgroundColor : "#ffffff"
scalesProperties.fontSize: 11
scalesProperties.lineColor : "#555"
scalesProperties.textColor : "#555"
scalesProperties.scaleSeriesOnly : false
scalesProperties.showSeriesLastValue: true
scalesProperties.showSeriesPrevCloseValue: false
scalesProperties.showStudyLastValue: false
scalesProperties.showStudyPlotLabels: false
scalesProperties.showSymbolLabels: false

timeScale.rightOffset: 5

timezone: "Etc/UTC" # See supported timezones list (at Symbology#timezone page) for available values


// Series style. See the supported values below
// Bars = 0
// Candles = 1
// Line = 2
// Area = 3
// Heiken Ashi = 8
// Hollow Candles = 9
// Renko = 4
// Kagi = 5
// Point&Figure = 6
// Line Break = 7
// Baseline = 10
mainSeriesProperties.style: 1

mainSeriesProperties.showCountdown: true
mainSeriesProperties.visible:true
mainSeriesProperties.showPriceLine: true
mainSeriesProperties.priceLineWidth: 1
mainSeriesProperties.lockScale: false
mainSeriesProperties.minTick: "default"

mainSeriesProperties.priceAxisProperties.autoScale:true             (see #749)
mainSeriesProperties.priceAxisProperties.autoScaleDisabled:false    (see #749)
mainSeriesProperties.priceAxisProperties.percentage:false
mainSeriesProperties.priceAxisProperties.percentageDisabled:false
mainSeriesProperties.priceAxisProperties.log:false
mainSeriesProperties.priceAxisProperties.logDisabled:false

symbolWatermarkProperties.color : "rgba(0, 0, 0, 0.00)"

// Different chart types defaults

// Candles styles
mainSeriesProperties.candleStyle.upColor: "#6ba583"
mainSeriesProperties.candleStyle.downColor: "#d75442"
mainSeriesProperties.candleStyle.drawWick: true
mainSeriesProperties.candleStyle.drawBorder: true
mainSeriesProperties.candleStyle.borderColor: "#378658"
mainSeriesProperties.candleStyle.borderUpColor: "#225437"
mainSeriesProperties.candleStyle.borderDownColor: "#5b1a13"
mainSeriesProperties.candleStyle.wickUpColor: 'rgba( 115, 115, 117, 1)'
mainSeriesProperties.candleStyle.wickDownColor: 'rgba( 115, 115, 117, 1)'
mainSeriesProperties.candleStyle.barColorsOnPrevClose: false

// Hollow Candles styles
mainSeriesProperties.hollowCandleStyle.upColor: "#6ba583"
mainSeriesProperties.hollowCandleStyle.downColor: "#d75442"
mainSeriesProperties.hollowCandleStyle.drawWick: true
mainSeriesProperties.hollowCandleStyle.drawBorder: true
mainSeriesProperties.hollowCandleStyle.borderColor: "#378658"
mainSeriesProperties.hollowCandleStyle.borderUpColor: "#225437"
mainSeriesProperties.hollowCandleStyle.borderDownColor: "#5b1a13"
mainSeriesProperties.hollowCandleStyle.wickColor: "#737375"

// Heiken Ashi styles
mainSeriesProperties.haStyle.upColor: "#6ba583"
mainSeriesProperties.haStyle.downColor: "#d75442"
mainSeriesProperties.haStyle.drawWick: true
mainSeriesProperties.haStyle.drawBorder: true
mainSeriesProperties.haStyle.borderColor: "#378658"
mainSeriesProperties.haStyle.borderUpColor: "#225437"
mainSeriesProperties.haStyle.borderDownColor: "#5b1a13"
mainSeriesProperties.haStyle.wickColor: "#737375"
mainSeriesProperties.haStyle.barColorsOnPrevClose: false

// Bar styles
mainSeriesProperties.barStyle.upColor: "#6ba583"
mainSeriesProperties.barStyle.downColor: "#d75442"
mainSeriesProperties.barStyle.barColorsOnPrevClose: false
mainSeriesProperties.barStyle.dontDrawOpen: false

// Line styles
mainSeriesProperties.lineStyle.color: "#0303F7"
mainSeriesProperties.lineStyle.linestyle: LINESTYLE_SOLID
mainSeriesProperties.lineStyle.linewidth: 1
mainSeriesProperties.lineStyle.priceSource: "close"

// Area styles
mainSeriesProperties.areaStyle.color1: "#606090"
mainSeriesProperties.areaStyle.color2: "#01F6F5"
mainSeriesProperties.areaStyle.linecolor: "#0094FF"
mainSeriesProperties.areaStyle.linestyle: LINESTYLE_SOLID
mainSeriesProperties.areaStyle.linewidth: 1
mainSeriesProperties.areaStyle.priceSource: "close"

// Baseline styles
mainSeriesProperties.baselineStyle.baselineColor: "rgba( 117, 134, 150, 1)"
mainSeriesProperties.baselineStyle.topFillColor1: "rgba( 83, 185, 135, 0.1)"
mainSeriesProperties.baselineStyle.topFillColor2: "rgba( 83, 185, 135, 0.1)"
mainSeriesProperties.baselineStyle.bottomFillColor1: "rgba( 235, 77, 92, 0.1)"
mainSeriesProperties.baselineStyle.bottomFillColor2: "rgba( 235, 77, 92, 0.1)"
mainSeriesProperties.baselineStyle.topLineColor: "rgba( 83, 185, 135, 1)"
mainSeriesProperties.baselineStyle.bottomLineColor: "rgba( 235, 77, 92, 1)"
mainSeriesProperties.baselineStyle.topLineWidth: 1
mainSeriesProperties.baselineStyle.bottomLineWidth: 1
mainSeriesProperties.baselineStyle.priceSource: "close"
mainSeriesProperties.baselineStyle.transparency: 50
mainSeriesProperties.baselineStyle.baseLevelPercentage: 50
```

#### LineStyles

```javascript
LINESTYLE_SOLID = 0
LINESTYLE_DOTTED = 1
LINESTYLE_DASHED = 2
LINESTYLE_LARGE_DASHED = 3
```
