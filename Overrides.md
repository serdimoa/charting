This topic contains chart properties description. These properties are treated
as customizable ones. Other properties customization is not supported. 

This file format:

	<property_path>: <default Charting Library value>

```
//	supported values: large, medium, small, tiny
volumePaneSize: "large"

//	fonts available in text editors (i.e., in `Text` drawing tool properties dialog)
editorFontsList: ['Verdana', 'Courier New', 'Times New Roman', 'Arial']

paneProperties.background: "#ffffff"
paneProperties.gridProperties.color: "#E6E6E6"
paneProperties.vertGridProperties.color: "#E6E6E6"
paneProperties.horzGridProperties.color: "#E6E6E6"
paneProperties.crossHairProperties.color: "#B7B7B7"

//	Margins (percent). Used for auto scaling.
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
scalesProperties.lineColor : "#555"
scalesProperties.textColor : "#555"
scalesProperties.scaleSeriesOnly : false


//	Series style. See the supported values below
//		bars = 0
//		candles = 1
//		line = 2
//		area = 3
//		heiken ashi = 8
//		hollow candles = 9
mainSeriesProperties.style: 1

mainSeriesProperties.showCountdown: true
mainSeriesProperties.showLastValue:true
mainSeriesProperties.visible:true
mainSeriesProperties.showPriceLine: true
mainSeriesProperties.priceLineWidth: 1
mainSeriesProperties.lockScale: false
mainSeriesProperties.minTick: "default"
mainSeriesProperties.extendedHours: false

mainSeriesProperties.priceAxisProperties.autoScale:true             (see #749)
mainSeriesProperties.priceAxisProperties.autoScaleDisabled:false    (see #749)
mainSeriesProperties.priceAxisProperties.percentage:false
mainSeriesProperties.priceAxisProperties.percentageDisabled:false
mainSeriesProperties.priceAxisProperties.log:false
mainSeriesProperties.priceAxisProperties.logDisabled:false

symbolWatermarkProperties.color : "rgba(0, 0, 0, 0.05)"

//	Different chart types defaults

//	Candles styles
mainSeriesProperties.candleStyle.upColor: "#6ba583"
mainSeriesProperties.candleStyle.downColor: "#d75442"
mainSeriesProperties.candleStyle.drawWick: true
mainSeriesProperties.candleStyle.drawBorder: true
mainSeriesProperties.candleStyle.borderColor: "#378658"
mainSeriesProperties.candleStyle.borderUpColor: "#225437"
mainSeriesProperties.candleStyle.borderDownColor: "#5b1a13"
mainSeriesProperties.candleStyle.wickColor: "#737375"
mainSeriesProperties.candleStyle.barColorsOnPrevClose: false

//	Hollow Candles styles
mainSeriesProperties.hollowCandleStyle.upColor: "#6ba583"
mainSeriesProperties.hollowCandleStyle.downColor: "#d75442"
mainSeriesProperties.hollowCandleStyle.drawWick: true
mainSeriesProperties.hollowCandleStyle.drawBorder: true
mainSeriesProperties.hollowCandleStyle.borderColor: "#378658"
mainSeriesProperties.hollowCandleStyle.borderUpColor: "#225437"
mainSeriesProperties.hollowCandleStyle.borderDownColor: "#5b1a13"
mainSeriesProperties.hollowCandleStyle.wickColor: "#737375"

//	Heiken Ashi styles
mainSeriesProperties.haStyle.upColor: "#6ba583"
mainSeriesProperties.haStyle.downColor: "#d75442"
mainSeriesProperties.haStyle.drawWick: true
mainSeriesProperties.haStyle.drawBorder: true
mainSeriesProperties.haStyle.borderColor: "#378658"
mainSeriesProperties.haStyle.borderUpColor: "#225437"
mainSeriesProperties.haStyle.borderDownColor: "#5b1a13"
mainSeriesProperties.haStyle.wickColor: "#737375"
mainSeriesProperties.haStyle.barColorsOnPrevClose: false

//	Bars styles
mainSeriesProperties.barStyle.upColor: "#6ba583"
mainSeriesProperties.barStyle.downColor: "#d75442"
mainSeriesProperties.barStyle.barColorsOnPrevClose: false
mainSeriesProperties.barStyle.dontDrawOpen: false

//	Line styles
mainSeriesProperties.lineStyle.color: "#0303F7"
mainSeriesProperties.lineStyle.linestyle: CanvasEx.LINESTYLE_SOLID
mainSeriesProperties.lineStyle.linewidth: 1
mainSeriesProperties.lineStyle.priceSource: "close"

//	Area styles
mainSeriesProperties.areaStyle.color1: "#606090"
mainSeriesProperties.areaStyle.color2: "#01F6F5"
mainSeriesProperties.areaStyle.linecolor: "#0094FF"
mainSeriesProperties.areaStyle.linestyle: CanvasEx.LINESTYLE_SOLID
mainSeriesProperties.areaStyle.linewidth: 1
mainSeriesProperties.areaStyle.priceSource: "close"
```

####Customization of Overlay Symbols

```
study_Overlay@tv-basicstudies.style: (bars = 0, candles = 1, line = 2, area = 3, heiken ashi = 8, hollow candles = 9)
study_Overlay@tv-basicstudies.showPriceLine: boolean

study_Overlay@tv-basicstudies.candleStyle.upColor: color
study_Overlay@tv-basicstudies.candleStyle.downColor: color
study_Overlay@tv-basicstudies.candleStyle.drawWick: boolean
study_Overlay@tv-basicstudies.candleStyle.drawBorder: boolean
study_Overlay@tv-basicstudies.candleStyle.borderColor: color
study_Overlay@tv-basicstudies.candleStyle.borderUpColor: color
study_Overlay@tv-basicstudies.candleStyle.borderDownColor: color
study_Overlay@tv-basicstudies.candleStyle.wickColor: color
study_Overlay@tv-basicstudies.candleStyle.barColorsOnPrevClose: boolean

study_Overlay@tv-basicstudies.hollowCandleStyle.upColor: color
study_Overlay@tv-basicstudies.hollowCandleStyle.downColor: color
study_Overlay@tv-basicstudies.hollowCandleStyle.drawWick: boolean
study_Overlay@tv-basicstudies.hollowCandleStyle.drawBorder: boolean
study_Overlay@tv-basicstudies.hollowCandleStyle.borderColor: color
study_Overlay@tv-basicstudies.hollowCandleStyle.borderUpColor: color
study_Overlay@tv-basicstudies.hollowCandleStyle.borderDownColor: color
study_Overlay@tv-basicstudies.hollowCandleStyle.wickColor: color
study_Overlay@tv-basicstudies.hollowCandleStyle.barColorsOnPrevClose: boolean

study_Overlay@tv-basicstudies.barStyle.upColor: color
study_Overlay@tv-basicstudies.barStyle.downColor: color
study_Overlay@tv-basicstudies.barStyle.barColorsOnPrevClose: boolean
study_Overlay@tv-basicstudies.barStyle.dontDrawOpen: boolean


study_Overlay@tv-basicstudies.lineStyle.color: color
study_Overlay@tv-basicstudies.lineStyle.linestyle: (solid = 0; dotted = 1; dashed = 2; large dashed = 3)
study_Overlay@tv-basicstudies.lineStyle.linewidth: integer
study_Overlay@tv-basicstudies.lineStyle.priceSource: open/high/low/close
study_Overlay@tv-basicstudies.lineStyle.styleType: (bars = 0, candles = 1, line = 2, area = 3, heiken ashi = 8, hollow candles = 9)

study_Overlay@tv-basicstudies.areaStyle.color1: color
study_Overlay@tv-basicstudies.areaStyle.color2: color
study_Overlay@tv-basicstudies.areaStyle.linecolor: color
study_Overlay@tv-basicstudies.areaStyle.linestyle: (solid = 0; dotted = 1; dashed = 2; large dashed = 3)
study_Overlay@tv-basicstudies.areaStyle.linewidth: integer
study_Overlay@tv-basicstudies.areaStyle.priceSource: open/high/low/close
```