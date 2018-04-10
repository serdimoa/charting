Trading Primitives is a low-level mechanism that is designed to give you the most detailed control over trading primitives behavior.

## Generic data

### Properties

You can use special value `inherit` for some properties of the Trading Primitives. This will make the Library use its default value for this property.

You can enable the `trading_options` feature to show Trading GUI controls in the chart properties window.

You can’t control the visibility of a specific object. Properties of positions, orders and executions can be changed in the Trading tab of the Chart Properties window.

### Colors and Fonts

You can use following string formats for setting the colors:

1. `#RGB`
1. `#RRGGBB`
1. `rgb(red, green, blue)`
1. `rgba(red, green, blue, alpha)`

You can use the following string format for setting the fonts: `<bold> <italic> <size>pt <family>`.

### Line Styles

The following line styles are supported:

Style|Value
---|---
Solid|0
Dotted|1
Dashed|2

### Callbacks

1. You can use `this` keyword to access an API-object from within the callback function
1. You can pass two arguments to callback registration function - in this case first argument is an object which will be passed as first argument to callback function.
1. If callback isn’t set, then respective button will be hidden (affects `onReverse`, `onClose` and `onCancel` callbacks).
