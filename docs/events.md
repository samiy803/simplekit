# Event System

SimpleKit separates raw browser input from toolkit events using a small
windowing system abstraction. This allows the same UI logic to work in both
canvas and imperative modes.

## Fundamental Events

The windowing system (see `windowing-system` module) listens for DOM events and
creates **FundamentalEvents**. These represent low level actions such as
`mousedown`, `mouseup`, `mousemove`, `keydown`, `keyup` and `resize`. Each event
contains a `timeStamp` along with coordinates or key data. Fundamental events
are placed on a shared queue which the toolkit consumes each animation frame.

A helper `coalesceEvents` function merges multiple mouse move or resize events so
that only the latest of each type is processed.

## SKEvent Hierarchy

The toolkit defines several classes which describe higher level events passed to
widgets and global listeners.

- `SKEvent` – Base class containing the event type, timestamp and optional
  source object.
- `SKMouseEvent` – Extends `SKEvent` and includes `x` and `y` coordinates.
- `SKKeyboardEvent` – Extends `SKEvent` and carries an optional key string.
- `SKResizeEvent` – Reports changes to the canvas size with `width` and `height`.

All event objects are exported from the `events` module and are used throughout
SimpleKit.

## Event Translators

Fundamental events are transformed into toolkit events by **Event Translators**.
Each translator implements an `update(fe: FundamentalEvent)` function that either
returns an `SKEvent` or `undefined` if no event should be generated.

Four translators are provided:

- `fundamentalTranslator` – Converts fundamental events directly to the matching
  `SKEvent` subclass. For example a `mousemove` fundamental event becomes an
  `SKMouseEvent`.
- `clickTranslator` – Detects mouse down and up pairs within a short time and
  distance to emit `click` events.
- `dblclickTranslator` – Uses an internal click translator to detect two clicks
  occurring within a configurable time threshold, emitting a `dblclick` event.
- `dragTranslator` – Watches mouse movement after a press to generate `dragstart`,
  `drag` and `dragend` events once movement exceeds a threshold.

Applications can add their own translators using `addSKEventTranslator()` in
both runtime modes.

## Dispatching Events

In **canvas mode** all translated events are delivered to the global event
listener via `setSKEventListener`. Applications typically switch on the event
`type` to perform custom logic.

In **imperative mode** events are first routed to the widget tree. The mouse and
keyboard dispatchers manage focus and hit testing:

1. `mouseDispatch` determines the element route under the pointer, sends
   `mouseenter`/`mouseexit` transitions and performs capture/bubble propagation.
2. `keyboardDispatch` forwards keyboard events to the currently focused widget.

Widgets implement `handleMouseEvent` and `handleKeyboardEvent` to react to these
callbacks. Global listeners registered with `setSKEventListener` still receive
all events after widget dispatch is complete.

## Requesting Focus

Imperative mode exposes `requestMouseFocus` and `requestKeyboardFocus` helpers in
the `dispatch` module. Widgets such as `SKButton` and `SKTextfield` call these
methods when appropriate so they receive subsequent events exclusively until the
operation is finished or focus is cleared.

## Custom Events

Widgets may emit their own high level events. For example `SKButton` emits an
`action` event when clicked and `SKTextfield` emits `textchanged` whenever its
text content updates. These events flow through the same dispatch mechanism and
can be observed using `addEventListener` on the widget instance.
