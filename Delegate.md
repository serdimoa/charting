Delegate object is returned by some [Trading Terminal](Trading-Terminal) methods and is is required to use an [Account Manager](Account-Manager). Using this object you can get updated and update of an event.

### subscribe(object, member)

1. `object` is an owner of member, it can be null for a function
1. `member` is a method of object

Subscribes object::member to the event.

### unsubscribe(object, member)

Unsubscribes object::member from the event.

Use the same object and member that you used in `subscribe` function to unsubscribe from the event.
