## Displaying your data as an indicator

The instruction below explains how to display chart data as an indicator. Please follow these steps.

1. Come up with a new ticker name to display your data and set up your server to return valid SymbolInfo for this new ticker.
1. Also, set up the server to return valid historical data for this ticker.
1. Use the indicator template and complete the following fields: name, descriptions, and the ticker. Modify the default style of the indicator if required.

    ```javascript
    {
        // Replace the <study name> with your study name
        // The name will be used internally by the Charting Library
        name: "<study name>",
        metainfo: {
            "_metainfoVersion": 40,
            "id": "<study name>@tv-basicstudies-1",
            "scriptIdPart": "",
            "name": "<study name>",

            // This description will be displayed in the Indicators window
            // It is also used as a "name" argument when calling the createStudy method
            "description": "<study description>",

            // This description will be displayed on the chart
            "shortDescription": "<short study description>",

            "is_hidden_study": true,
            "is_price_study": true,
            "isCustomIndicator": true,

            "plots": [{"id": "plot_0", "type": "line"}],
            "defaults": {
                "styles": {
                    "plot_0": {
                        "linestyle": 0,
                        "visible": true,

                        // Plot line width.
                        "linewidth": 2,

                        // Plot type:
                        //    1 - Histogramm
                        //    2 - Line
                        //    3 - Cross
                        //    4 - Area
                        //    5 - Columns
                        //    6 - Circles
                        //    7 - Line With Breaks
                        //    8 - Area With Breaks
                        "plottype": 2,

                        // Show price line?
                        "trackPrice": false,

                        // Plot transparency, in percent.
                        "transparency": 40,

                        // Plot color in #RRGGBB format
                        "color": "#0000FF"
                    }
                },

                // Precision of the study's output values
                // (quantity of digits after the decimal separator).
                "precision": 2,

                "inputs": {}
            },
            "styles": {
                "plot_0": {
                    // Output name will be displayed in the Style window
                    "title": "-- output name --",
                    "histogramBase": 0,
                }
            },
            "inputs": [],
        },

        constructor: function() {
            this.init = function(context, inputCallback) {
                this._context = context;
                this._input = inputCallback;

                // Define the symbol to be plotted.
                // Symbol should be a string.
                // You can use PineJS.Std.ticker(this._context) to get the selected symbol's ticker.
                // For example,
                //    var symbol = "AAPL";
                //    var symbol = "#EQUITY";
                //    var symbol = PineJS.Std.ticker(this._context) + "#TEST";
                var symbol = "<TICKER>";
                this._context.new_sym(symbol, PineJS.Std.period(this._context), PineJS.Std.period(this._context));
            };

            this.main = function(context, inputCallback) {
                this._context = context;
                this._input = inputCallback;

                this._context.select_sym(1);

                // You can use following built-in functions in PineJS.Std object:
                //    open, high, low, close
                //    hl2, hlc3, ohlc4
                var v = PineJS.Std.close(this._context);
                return [v];
            }
        }
    }
    ```

1. Save the indicator into the custom indicators file with the following structure:

    ```javascript
    __customIndicators = [
        // *** your indicator object, created from the template ***
    ];
    ```

    Note, that indicators file is a JavaScript source file that only defines an array of indicator objects. You are able to add several indicators to this file. You are also able to add the indicators that we compiled for you.

1. Use [indicators_file_name](Widget-Constructor#indicators_file_name) option of widget's constructor to load custom indicators from the indicators file.
1. Update your widget's initialization code to [create](Chart-Methods#createstudyname-forceoverlay-lock-inputs-callback-overrides-options) this indicator when the chart is ready.

## Examples

1. Add the indicator to the Charting Library using [indicators_file_name](Widget-Constructor#indicators_file_name) option.
1. Change your widget's initialization code. Here is an example.

    ```javascript
    widget = new TradingView.widget(/* ... */);

    widget.onChartReady(function() {
        widget.chart().createStudy('<indicator-name>', false, true);
    });
    ```

### Requesting data for another ticker

Let's assume that you wish to dispaly the equity curve on the chart. You will have to do the following.

* Create a name for the new ticker, e.g. `#EQUITY`. You can use any name that you can think of.
* Change your server's code to validate this symbol. The minimum amount of symobol information should be returned for this ticker.
* Make the server return valid historical data for this ticker just as you return the historcal data for the generic symbols such as AAPL.
* Modify the indicator template mentioned above and create the indicators file (or add a new indicator to the existing indicators file). Here is an example:

```javascript
__customIndicators = [
    {
        name: "Equity",
        metainfo: {
            "_metainfoVersion": 40,
            "id": "Equity@tv-basicstudies-1",
            "scriptIdPart": "",
            "name": "Equity",
            "description": "Equity",
            "shortDescription": "Equity",

            "is_hidden_study": true,
            "is_price_study": true,
            "isCustomIndicator": true,

            "plots": [{"id": "plot_0", "type": "line"}],
            "defaults": {
                "styles": {
                    "plot_0": {
                        "linestyle": 0,
                        "visible": true,

                        // Make the line thinner
                        "linewidth": 1,

                        // Plot type is Line
                        "plottype": 2,

                        // Show price line
                        "trackPrice": true,

                        "transparency": 40,

                        // Set the plotted line color to dark red
                        "color": "#880000"
                    }
                },

                // Precision is set to one digit, e.g. 777.7
                "precision": 1,

                "inputs": {}
            },
            "styles": {
                "plot_0": {
                    // Output name will be displayed in the Style window
                    "title": "Equity value",
                    "histogramBase": 0,
                }
            },
            "inputs": [],
        },

        constructor: function() {
            this.init = function(context, inputCallback) {
                this._context = context;
                this._input = inputCallback;

                var symbol = "#EQUITY";
                this._context.new_sym(symbol, PineJS.Std.period(this._context), PineJS.Std.period(this._context));
            };

            this.main = function(context, inputCallback) {
                this._context = context;
                this._input = inputCallback;

                this._context.select_sym(1);

                var v = PineJS.Std.close(this._context);
                return [v];
            }
        }
    }
];
```

### Coloring bars

```javascript
__customIndicators = [
    {
        name: "Bar Colorer Demo",
        metainfo: {
            _metainfoVersion: 42,

            id: "BarColoring@tv-basicstudies-1",

            name: "BarColoring",
            description: "Bar Colorer Demo",
            shortDescription: "BarColoring",
            scriptIdPart: "",
            is_price_study: true,
            is_hidden_study: false,
            isCustomIndicator: true,

            isTVScript: false,
            isTVScriptStub: false,
            defaults: {
                precision: 4,
                palettes: {
                    palette_0: {
                        // palette colors
                        // change it to the default colors that you prefer,
                        // but note that the user can change them in the Style tab
                        // of indicator properties
                        colors: [
                            { color: "#FFFF00" },
                            { color: "#0000FF" }
                        ]
                    }
                }
            },
            inputs: [],
            plots: [{
                id: "plot_0",

                // plot type should be set to 'bar_colorer'
                type: "bar_colorer",

                // this is the name of the palette that is defined
                // in 'palettes' and 'defaults.palettes' sections
                palette: "palette_0"
            }],
            palettes: {
                palette_0: {
                    colors: [
                        { name: "Color 0" },
                        { name: "Color 1" }
                    ],

                    // the mapping between the values that
                    // are retured by the script and palette colors
                    valToIndex: {
                        100: 0,
                        200: 1
                    }
                }
            }
        },
        constructor: function() {
            this.main = function(context, input) {
                this._context = context;
                this._input = input;

                var valueForColor0 = 100;
                var valueForColor1 = 200;

                // perform your calculations here and return one of the constants
                // that is specified as a key in 'valToIndex' mapping
                var result =
                    Math.random() * 100 % 2 > 1 ? // we randomly select one of the color values
                        valueForColor0 : valueForColor1;

                return [result];
            }
        }
    }
];
```
