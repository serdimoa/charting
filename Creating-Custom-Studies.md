## How to show your data as an indicator

Here is an instruction for the case if you have some data which you want to show on the chart like an indicator.

Please follow these few steps:

1. Contrive a new ticker for your data and set up your server to return valid SymbolInfo for this ticker.
1. Also set up the server to return valid history data for this ticker.
1. Use the following indicator template and fill in all placeholder values: name, descriptions, and the ticker. Also modify the default style of plot, if required.

    ```javascript
    {
        // Replace the <study name> with your study name
        // It will be used internally by the Charting Library
        name: "<study name>",
        metainfo: {
            "_metainfoVersion": 40,
            "id": "<study name>@tv-basicstudies-1",
            "scriptIdPart": "",
            "name": "<study name>",

            // This description will be displayed in the Indicators window
            // It is also used as a "name" argument when calling createStudy method
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
        // *** your indicator object, created from template ***
    ];
    ```

    Note, that indicators file is a JavaScript source file that just defines an array of indicator objects. So, you can place several indicators in it, or combine them with the ones we compiled for you.

1. Use [indicators_file_name](Widget-Constructor.md#indicators_file_name) option of widget's constructor to load custom indicators from the indicators file.
1. Update your widget's initialization code to [create](Chart-Methods.md#createstudyname-forceoverlay-lock-inputs-callback-overrides-options) this indicator when chart is ready.

## Examples

*How to use the examples?*

1. Plug the indicator into Charting Library using [indicators_file_name](Widget-Constructor.md#indicators_file_name) option.
1. Change your widget's initialization code. Add something like the following:

    ```javascript
    widget = new TradingView.widget(/* ... */);

    widget.onChartReady(function() {
        widget.chart().createStudy('<indicator-name>', false, true);
    });
    ```

### Requesting data for another ticker

Assume you want to show user's equity curve on his charts. You will have to do the following:

* Create a name for the new ticker. Assume it will be `#EQUITY` ticker. You can use any name you can imagine.
* Change your server's code to resolve this ticker as a valid symbol. Return the minimal valuable SymbolInfo for this case.
* Make the server to return valid history for this ticker. I.e., the server could ask your database for equity history and return this data just as you return the history for generic symbols (like, say, `AAPL`).
* Adopt the indicator template mentioned above and create the indicators file (or add a new indicator to the existing indicators file). This could be something like the following:

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

                        // Set the dark red color for the plot line
                        "color": "#880000"
                    }
                },

                // Precision is one digit, like 777.7
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
                        // change it to the defaults you prefer,
                        // but note that user can change them in Style tab
                        // of indicator property page
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

                // this is the reference to the palette
                // defined in 'palettes' and 'defaults.palettes' sections
                palette: "palette_0"
            }],
            palettes: {
                palette_0: {
                    colors: [
                        { name: "Color 0" },
                        { name: "Color 1" }
                    ],

                    // mapping of values returned by the script
                    // to the specific colors in the palette
                    // value can be arbitrary number constant
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
                // specified as key in 'valToIndex' mapping
                var result =
                    Math.random() * 100 % 2 > 1 ? // we just randomly select one of the color values
                        valueForColor0 : valueForColor1;

                return [result];
            }
        }
    }
];
```
