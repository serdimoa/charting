## Shape API

### isSelectionEnabled()

Returns `true` if the shape cannot be selected by a user.

### setSelectionEnabled(enable)

1. `enable` - `true` or `false`

Enables or disables shape selection (see `disableSelection` option of `createMultipointShape`).

### isSavingEnabled()

Returns `true` if the shape is not saved on the chart.

### setSavingEnabled(enable)

1. `enable` - `true` or `false`

Enables or disables saving of the shape in the chart layout (see `disableSave` option of `createMultipointShape`).

### isShowInObjectsTreeEnabled()

Returns `true` if the shape is displayed in the Objects Tree dialog.

### setShowInObjectsTreeEnabled(enabled)

1. `enabled` - `true` or `false`

Enables or disables displaying of the shape in the Objects Tree dialog.

### isUserEditEnabled()

Returns `true` if a user can remove/change/hide the shape.

### setUserEditEnabled(enabled)

1. `enabled` - `true` or `false`

Enables or disables removing/changing/hiding of the shape by a user

### bringToFront()

Places the line tool on top of all other chart objects.

### sendToBack()

Places the line tool behind all other chart objects.

### getProperties()

Gets all the properties of the shape.

### setProperties(properties)

1. `properties` - an object with new properties. It should have the same structure as an object from [getProperties](#getproperties).
    It can only include the properties that you want to override.

Sets the properties of the shape.

### getPoints()

Returns the points of the shape - an array of the [PricedPoint](#pricedpoint) objects.

### setPoints(points)

1. `points` - an array with the new points for the shape. The format of each shape is the same as `points` argument from [createMultipointShape](Chart-Methods#createmultipointshapepoints-options) method.

Set the new points of the shape.

## Primitive Types

### PricedPoint

An object with the following keys:

* `price` - price value of the point
* `time` - time of the point
