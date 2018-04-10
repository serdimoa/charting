Subscription object is returned by [Chart Methods](Chart-Methods). This object allows you to subscribe and unsubscribe to a chart event. It has two methods:

### subscribe(object, method, singleshot)

1. `object` is a context to be used as `this` pointer for the `method` function. Use `null` if you don't need the context.
1. `method` is a function to be called when the event happens.
1. `singleshot` is an optional argument. Set it to `true` if you wish to be unsubscribed automatically when the event happens for the first time.

### unsubscribe(object, method)

Use the same `object` and `method` that you used in the `subscribe` function to unsubscribe from the event.

### unsubscribeAll(object)

Use the same `object` that you used in `subscribe` function to unsubscribe `object` from all events.
