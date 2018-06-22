## Pane API

### hasMainSeries()

Returns `true` if the pane contains the main series.

### getLeftPriceScale()

Returns an instance of the [PriceScaleApi](Price-Scale-Api) that allows you to interact with left price scale.

### getRightPriceScale()

Returns an instance of the [PriceScaleApi](Price-Scale-Api) that allows you to interact with right price scale.

### getMainSourcePriceScale()

Returns:

* an instance of the [PriceScaleApi](Price-Scale-Api) that allows you to interact with the price scale of the main source
* `null` if the main source is not attached to any price scale (it is in 'No Scale' mode)
