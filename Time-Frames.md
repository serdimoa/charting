You can see the toolbar at the bottom of the chart. Each of those buttons on the left side switches the chart time frame. Switching time frame means:

1. Switch the chart resolution
1. Force the bars to scale horizontally in order to cover the entire requested date/time range.

I.e., clicking `1Y` will make the chart switch the resolution to `1D` and scale it accordingly to display daily bars for the entire year. Each time frame has its own chart resolution. Here it the list of default time frames:

Time Frame|Chart Resolution
---|---
5Y|W
1Y|D
6M|120
3M|60
1M|30
5D|5
1D|1

You can customize default time frames using the [time_frames](Widget-Constructor#time_frames) argument of the widget constructor.

**Remark**: Time frames that require resolutions which are not available for a particular symbol will be hidden.
