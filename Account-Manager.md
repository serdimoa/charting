:chart: All content on this page is relevant to [Trading Terminal](Trading-Terminal) only.

Account Manager is an interactive table that displays trading information.
It includes 3 pages: orders/positions and account information.

To create an account manager you will need to describe columns of each page and provide data.

Remark 1. [Broker API](Broker-API) should implement [accountManagerInfo](Broker-API#accountmanagerinfo)

<!--
Be sure that you have the following structure:
## Fields group name
### Field name
### Field name
#### Field subitem description
-->

# Account Manager Meta Information

The following information should be returned by [accountManagerInfo](Broker-API#accountManagerInfo).

## Account Manager header

Account Manager's header includes the name of the broker and an account name or a list of accounts.

### accountTitle: String

### accountsList: array of AccountInfo

### account: [WatchedValue](WatchedValue) of AccountInfo

`AccountInfo` is an object with the following keys:

1. `id` - account id
1. `name` - account name
1. `currency` - account currency

If the `currency` key is not set, `USD` will be used as a default value.

## Orders Page

### orderColumns: array of [Column](#column-description)

Columns description that you want to be displayed on the Orders page.
You can display any field of an [order](Trading-Objects-and-Constants#order) or add your own fields to an order object and display them.

### orderColumnsSorting: [SortingParameters](#sortingparameters)

Optional sorting of the table. If it is not set, the table is sorted by the first column.

### possibleOrderStatuses: array of [OrderStatus](Trading-Objects-and-Constants#orderstatus)

Optional list of statuses to be used in the orders filter. Default list is used if it hasn't been set.

### historyColumns: array of [Column](#column-description)

History page will be displayed if it exists. All orders from previous sessions will be shown in the History.

### historyColumnsSorting: [SortingParameters](#sortingparameters)

Optional sorting of the table. If it is not set, the table is sorted by the first column.

## Positions Page

### positionColumns: array of [Column](#column-description)

You can display any field of a [position](Trading-Objects-and-Constants#position) or add your own fields to a position object and display them.

## Additional Pages (e.g. Account Summary)

### pages: array of [Page](#page)

You can add new tabs in the Account Manager by using `pages`. Each tab is a set of tables.

#### Page

`Page` is a description of an additional Account Manager tab. It is an object with the following fields:

1. `id`: String

    Unique identifier of a page

1. `title`: String

    Page title. It is the tab name.

1. `tables`: Array of [Table](#table).

    It is possible to display one or more tables in this tab.

#### Table

You can add one or more tables to a [Page](#page).
Account Summary table metainfo is an object with the following fields:

1. `id`: String

    Unique identifier of a table.

1. `title`: String

    Optional title of a table.

1. `columns`: array of [Column](#column-description)

1. `getData`: Promise

    This function is used to request table data. It should return Promise (or Deferred) and resolve it with an array of data rows.

    Each row is an object. Keys of this object are column names with the corresponding values.

    There is a predefined field `isTotalRow` which can be used to mark a row that should be at the bottom of a table.

1. `changeDelegate` : [Delegate](Delegate)

    This delegate is used to watch the data changes and update the table. Pass new account manager data to `fire` method of the delegate.

1. `initialSorting`: [SortingParameters](#sortingparameters)

    Optional sorting of the table. If it is not set, the table is sorted by the first column.

**NOTE**: if you have more than 1 row in a table and want to update a row using `changeDelegate` make sure that you have `id` field in each row to identify it.
It is not necessary if you have only 1 row in a table.

#### SortingParameters

Object with the following properties:

    - `columnId` - `id` or `property` of the column that will be used for sorting.
    - `asc` - (optional, default `false`) - If it is `true`, then initial sorting will be in ascending order.

## Formatters

### customFormatters: array of a column formatter description

Optional array to define custom formatters. Each description is an object with the following fields:

- `name`: unique identifier of a formatter.

- `format(options)`: function that is used for formatting of a cell value. `options` is an object with the following keys:
    1. `value` - value to be formatted
    1. `priceFormatter` - standard formatter for price. You can use method `format(price)` to prepare price value.
    1. `prevValue` - optional field. It is a previous value so you can compare and format accordingly. It exists if current column has the `highlightDiff: true` key.
    1. `row` - object with all key/value pairs from the current row

## Column description

The most valuable part of Account Manager description is a description of its columns.

### label

Column title. It will be displayed in the table's header row.

### className

Optional `className` is added to the html tag of each value cell.
You can use it to customize table's style.

Here is a list of predefined classes:

| class name   |   description  |
|--------------|----------------|
| `tv-data-table__cell--symbol-cell` | Special formatter for a symbol field |
| `tv-data-table__cell--right-align` | It aligns cell value to the right |
| `tv-data-table__cell--buttons-cell` | Cell with a buttons |

### formatter

Name of the formatter to be used for data formatting. If `formatter` is not set the value is displayed as is.
Formatter can be a default or a custom one.

Here is the list of default formatters:

| name | description |
| ---- | ----------- |
| `symbol` | It is used for a symbol field. It displays `brokerSymbol`, but when you click on a symbol the chart changes according to the `symbol` field. `property` key is ignored.|
| `side` | It is used to display the side: Sell or Buy. |
| `type` | It is used to display the type of order: Limit/Stop/StopLimit/Market. |
| `formatPrice` | Symbol price formatting. |
| `formatQuantity` | Displays an integer or floating point quantity, separates thousands groups with a space. |
| `formatPriceForexSup` | The same as `formatPrice`, but it makes the last character of the price superscripted. It works only if instrument type is set to `forex`.|
| `status` | It is used to format the `status`. |
| `date` | Displays the date or time. |
| `localDate` | Displays the local date or time. |
| `fixed` | Displays a number with 2 decimal places. |
| `pips` | Displays a number with 1 decimal place. |
| `profit` | Displays profit. It also adds the `+` sign, separates thousands and changes the cell text color to red or green. |

There are some special formatters that are used to add buttons to the table:

`orderSettings` adds Modify/Cancel buttons to the orders tab. Always set `modificationProperty` value to `status` for this formatter.

`posSettings` adds Edit/Close buttons to the Positions/Net Positions tab

`tradeSettings` adds Edit/Close buttons to the Individual Positions tab. Always set `modificationProperty` value to `canBeClosed` for this formatter.

### property

`property` is a data object key that is used to get the data to display in a table.

### sortProp

Optional `sortProp` is a data object key that is used for data sorting.

### modificationProperty

If optional `modificationProperty` is set then the change of its value updates the table cell.

### notSortable

Optional `notSortable` can be set to prevent column sorting.

### help

`help` is a tooltip string for the column.

### highlightDiff

`highlightDiff` can be set with `formatPrice` and `formatPriceForexSup` formatters only to highlight the changes of the field.

### fixedWidth

If it is `true` then the column width will not be changed when the cell value is decreased.

### supportedStatusFilters

An optional numeric array of order statuses that is applied to order columns only. If it is available then the column will be displayed in the specified tabs of the status filter only.

Here is the list of possible order statuses:

1. 0 - All
1. 1 - Canceled
1. 2 - Filled
1. 3 - Inactive
1. 5 - Rejected,
1. 6 - Working

## Context Menu

### contextMenuActions(e, activePageItems)

`e`: context object passed by a browser

`activePageItems`: array of `ActionMetainfo` items for the current page

Optional function to create a custom context menu.
It should return `Promise` that is resolved with an array of `ActionMetainfo`.
