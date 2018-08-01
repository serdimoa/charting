You can set the default style along with the input values for newly created indicators using the `studies_overrides` parameter.
Its value is expected to be an object where key is a path to a property that is going to be changed while value is the new value for it.

Example:

```javascript
studies_overrides: {
    "volume.volume.color.0": "#00FFFF",
    "volume.volume.color.1": "#0000FF",
    "volume.volume.transparency": 70,
    "volume.volume ma.color": "#FF0000",
    "volume.volume ma.transparency": 30,
    "volume.volume ma.linewidth": 5,
    "volume.show ma": true,
    "bollinger bands.median.color": "#33FF88",
    "bollinger bands.upper.linewidth": 7
}
```

In the example above, all created Bollinger Bands will have upper line width set to 7 (unless you create it through an API and specify a different value).

## Setting the study name

You should use the names of the studies in the Insert Study dialog as they are but using lower case letters.

If you wish to override default EMA's length, try using `moving average exponential.length`.

The same logic applies to input names: use names as you see them in the Study Properties dialog (use lower case letters).

Example: `stochastic.smooth d`.

### Compare

You can customize new series that are added via `Compare`.

Use `compare.plot` to customize the line and `compare.source` to change the price source:

```javascript
"compare.plot.color": "#000000",
"compare.source": "high"
```

### Overlay

Starting from V 1.12 you may use the following properties to customize `Overlay`:

```javascript
Overlay.style: (bars = 0, candles = 1, line = 2, area = 3, heiken ashi = 8, hollow candles = 9)
Overlay.showPriceLine: boolean

Overlay.candleStyle.upColor: color
Overlay.candleStyle.downColor: color
Overlay.candleStyle.drawWick: boolean
Overlay.candleStyle.drawBorder: boolean
Overlay.candleStyle.borderColor: color
Overlay.candleStyle.borderUpColor: color
Overlay.candleStyle.borderDownColor: color
Overlay.candleStyle.wickColor: color
Overlay.candleStyle.barColorsOnPrevClose: boolean

Overlay.hollowCandleStyle.upColor: color
Overlay.hollowCandleStyle.downColor: color
Overlay.hollowCandleStyle.drawWick: boolean
Overlay.hollowCandleStyle.drawBorder: boolean
Overlay.hollowCandleStyle.borderColor: color
Overlay.hollowCandleStyle.borderUpColor: color
Overlay.hollowCandleStyle.borderDownColor: color
Overlay.hollowCandleStyle.wickColor: color
Overlay.hollowCandleStyle.barColorsOnPrevClose: boolean

Overlay.barStyle.upColor: color
Overlay.barStyle.downColor: color
Overlay.barStyle.barColorsOnPrevClose: boolean
Overlay.barStyle.dontDrawOpen: boolean

Overlay.lineStyle.color: color
Overlay.lineStyle.linewidth: integer
Overlay.lineStyle.priceSource: open/high/low/close
Overlay.lineStyle.styleType: (bars = 0, candles = 1, line = 2, area = 3, heiken ashi = 8, hollow candles = 9)

Overlay.areaStyle.color1: color
Overlay.areaStyle.color2: color
Overlay.areaStyle.linecolor: color
Overlay.areaStyle.linestyle: (solid = 0; dotted = 1; dashed = 2; large dashed = 3)
Overlay.areaStyle.linewidth: integer
Overlay.areaStyle.priceSource: open/high/low/close
```

## Syntax

Property path is a set of lower case identifiers split with a dot (`.`). Path formats are described below.

**Remark**: You can get an error if a plot/band/area/input name is the same .
In this case you can specify an exact destination that you want to change by adding `:plot`, `:band`, `:area` or `:input` to the path. (e.g. `short:plot.color`)

### Study input

Format: `indicator_name.input_name`

* **indicator_name**: use the name as you see it in the `Indicators` dialog.
* **input_name**: use the name as you see it in the indicator's properties dialog (for example, `show ma`)

Examples: `volume.show ma`, `bollinger bands.length`

### Plot property

Format: `indicator_name.plot_name.property_name`

* **indicator_name**:  < ... >
* **plot_name**: as you see it in the indicator's properties dialog (for example, `Volume` or `Plot`)
* **property_name**: one of the following:
  * **transparency**
  * **linewidth**
  * **plottype**. Supported plot types are:
    * `line`
    * `histogram`
    * `cross`
    * `area`
    * `columns`
    * `circles`
    * `line_with_breaks`
    * `area_with_breaks`

Examples: `volume.volume.transparency`, `bollinger bands.median.linewidth`

### Plot colors

Format: `indicator_name.plot_name.color<.color_index>`

* **indicator_name**:  < ... >
* **plot_name**:  < ... >
* **color**. It's just a keyword.
* **color_index** (optional): color index (if any). It's just an ordinal number of a color for this plot.
    I.e., to replace the color that is green by default for Volume, one should use `color_index = 1`.

**Remark 1**: `color.0` is a synonym of `color`. Paths such as `volume.volume.color.0` and `volume.volume.color` are treated the same.

**Remark 2**: The customization of area fill color and transparency is not supported currently.

**Limitations**:

* Only `#RRGGBB` format is supported for colors. Do not use a short format `#RGB`.
* Transparency varies and the range is [0..100]. 100 means plot is fully opaque.
* Thickness is an integer.

### Default precision

You can change the default precision of studies using the `name.precision` format starting from V 1.6.

Example: `"average true range.precision": 8`
