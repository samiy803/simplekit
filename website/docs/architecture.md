# SimpleKit Architecture

SimpleKit exposes two ways to build a user interface. **Canvas mode** provides a lightweight event loop and drawing canvas while **Imperative mode** adds a widget tree, layout system and event dispatch. Both modes share the same event and windowing subsystems.

## Windowing System

At the lowest level `src/windowing-system` simulates a browser window system. It converts DOM events into **FundamentalEvents** and calls the toolkit run loop once per animation frame. The entry point is `createWindowingSystem(runLoop)` which starts the loop and passes the event queue to your run loop handler. Global frame time is stored in `skTime`.

`coalesceEvents` is used to merge frequent events such as `mousemove` or `resize` so that only the most recent is processed each frame.

## Run Loop

Every mode in SimpleKit is driven by a simple run loop. The windowing system collects DOM events and queues them until `createWindowingSystem` calls your run loop handler. During each tick the following steps occur:

1. Pending events are translated and dispatched.
2. Animation callbacks update state based on the current `skTime` value.
3. In imperative mode, any invalidated layouts are recomputed.
4. Finally the draw callback renders the user interface to the canvas.

Because the loop is under your control it is easy to integrate SimpleKit with existing rendering or game loops.

## Event System

The `src/events` module defines the event classes used by the toolkit (`SKEvent`, `SKMouseEvent`, `SKKeyboardEvent`, `SKResizeEvent`). Fundamental events from the windowing system are converted into these high‑level events by **event translators**. The default translators support clicks, double clicks and drag sequences. Additional translators can be registered with `addSKEventTranslator()`.

## Canvas Mode

`src/canvas-mode.ts` exposes a minimal API for projects that only need a drawing canvas with translated events. Calling `startSimpleKit()` sets up the HTML page, creates a canvas and starts the windowing system. Applications register callbacks with:

- `setSKDrawCallback(gc)` – draw each frame.
- `setSKAnimationCallback(time)` – update animations.
- `setSKEventListener(event)` – receive all translated events.

This mode is useful for prototyping or teaching the event pipeline without the widget system.

## Imperative Mode

`src/imperative-mode.ts` builds upon canvas mode to provide an immediate mode widget toolkit. It manages a root widget, dispatches events to widgets and runs a layout pass when needed. The exported functions include everything from canvas mode plus:

- `setSKRoot(rootWidget)` – set the root of the widget tree.
- `sendSKEvent(event)` – inject custom events.
- `invalidateLayout()` – schedule a layout pass next frame.

Imperative mode dispatches mouse and keyboard events via modules in `src/dispatch`. Widgets can request focus through `requestKeyboardFocus()` and `requestMouseFocus()`.

## Widgets and Layout

Widgets reside in `src/widget`. `SKElement` is the abstract base class implementing sizing logic and event handling. Concrete widgets such as `SKButton`, `SKTextfield`, `SKLabel` and container widgets build upon it.

Layout strategies live under `src/layout`. The default `FixedLayout` positions children according to their `x` and `y` properties. Other strategies include `CentredLayout`, `FillRowLayout` and `WrapRowLayout`. Layout occurs in two stages:

1. `measure()` is called on each widget to determine its intrinsic size.
2. The container’s layout method sets the final position and size.

`invalidateLayout()` will trigger this process on the next frame.

## Utility Helpers

`src/utility` contains small helper functions for geometry calculations (distance, vectors), hit testing and text measurement. These are re‑exported by `simplekit/utility` and can be used directly by applications or custom widgets.

## Extending SimpleKit

The core library is intentionally small so that students can read through the code with ease. New widgets can be built by subclassing `SKElement` and implementing the sizing and drawing hooks. Likewise you can create custom layout strategies by exporting a class that implements the `Layout` interface. For specialised input, register additional event translators with `addSKEventTranslator` or send your own `SKEvent` instances with `sendSKEvent`.

## See Also

- [canvas-mode.md](./canvas-mode.md)
- [events.md](./events.md)
- [getting-started.md](./getting-started.md)