# Imperative UI Mode

The **imperative mode** provides SimpleKit's full widget system.  It manages event dispatch, layout and drawing for you.  Applications build a widget tree and let SimpleKit render it on a single canvas.

## Bootstrapping SimpleKit

```ts
import {
  startSimpleKit,
  setSKRoot,
  invalidateLayout,
  setSKEventListener,
  setSKAnimationCallback,
  addSKEventTranslator,
  sendSKEvent,
  requestKeyboardFocus,
  requestMouseFocus,
} from "simplekit/src/imperative-mode";
import { SKContainer, SKButton, SKLabel } from "simplekit/src/imperative-mode";
```

Call `startSimpleKit()` once.  It creates the canvas, sets up the simulated windowing system and begins the internal run loop.

## Building a widget tree

Widgets derive from `SKElement`.  The main container widget is `SKContainer`, which can hold children and apply a layout strategy.  SimpleKit includes several layouts such as `FixedLayout`, `CentredLayout`, `WrapRowLayout` and `FillRowLayout`.

```ts
const root = new SKContainer();
root.layoutMethod = new Layout.FixedLayout();

const button = new SKButton({ text: "Press" });
const label = new SKLabel({ text: "Hello" });
root.addChild(button);
root.addChild(label);

setSKRoot(root); // activates the widget tree
```

When widget properties change they call `invalidateLayout()` to request layout on the next frame.  Layout occurs after events and before drawing each frame.

## Events and focus

Mouse and keyboard events are translated to `SKMouseEvent` and `SKKeyboardEvent` and automatically dispatched down the widget tree.  Widgets can request focus using `requestMouseFocus(widget)` or `requestKeyboardFocus(widget)`.

Register an optional global listener with `setSKEventListener` to observe all events after widget dispatch.  Custom events can be queued with `sendSKEvent(event)`.

## Animation and drawing

Imperative mode clears the canvas and draws the widget tree each frame.  Provide an animation callback with `setSKAnimationCallback` if required.  Setting a custom draw callback is discouraged once a widget root is present (use `setSKDrawCallback` only for testing before calling `setSKRoot`).

## Typical application flow

1. Start the toolkit with `startSimpleKit()`.
2. Build widgets and set the root container.
3. Handle events or actions inside widgets or via a global listener.
4. Update state or perform animations each frame using `setSKAnimationCallback`.

The imperative mode is designed for teaching UI concepts.  It avoids CSS/HTML entirely, letting you focus on widget logic and layout while SimpleKit handles event processing and rendering.