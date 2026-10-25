---
title: "Events"
date: 2026-11-10T13:38:00+02:00
---

{{% notice warning %}}
Events can change without further notice as we settle in on a more "stable" form of handling / integrating them.
{{% /notice %}}

Events are used to decouple the Engelsystem flow logic and allow [plugins]({{% ref "plugins" %}}) to extend it
and add functionality.
They are implemented to be [PSR-14](https://www.php-fig.org/psr/psr-14/) compatible and thus easier to use.
Engelsystem events are used to notify about state changes (for example creating users, updating models etc.)
and as filters to allow for changing and extending data before it is used further (metrics, menu items etc.).

An event must extend `Engelsystem\Event\Event`, some helpers exist to make it easier like

* `StoppableEvent` extends the base Event and can be stopped as of PSR-14
* `NullEvent` default stoppable event with no additional data
* `DataEvent` if no more specific type exists but some data should to be provided
* `ModelEvent` an event that relates to a Engelsystem model
* `NewsEvent` used by events related to news entries
* `OAuth2Event` for oauth related events
* `LaravelEvent` is internally used to map Laravels model events to engelsystem events

and more, see [src/Events](https://github.com/engelsystem/engelsystem/tree/main/src/Events) for a list.

The `Engelsystem\Event\Event` base event provides the event name via `->getName()` which can be overwritten
by defining a `$name` attribute on any inheriting models like `NullEvent`;

## Listening

Event listeners can listen for a specific event or for multiply by matching a wildcard pattern like `model.message.*`.
`eloquent.*` is used to map Laravel model events to `model.*` events and should not be used directly.

Possible listening targets would be

```
model.message.created            After a message object got created
model.shifts.shifttype.deleting  Fired while a shift type is being deleted
shifts.deleting                  Gets a list of shifts that will be deleted to add worklogs etc.
oauth2.login                     Fired on usccessfull login / connection with an OAuth provider
```

where every model fires its corresponding events in `model.*` during and after the action of
`creating`/`created`, `updating`/`updated`, `deleting`/`deleted`, `saving`/`saved`.

## Handlers

An event handler can be any callback that expects an `Engelsystem\Event\Event` instance as its `$event` parameter
at the first position.

It can then react on the change and (if supported) update/filter data stored in the event before its
used in further processing.

