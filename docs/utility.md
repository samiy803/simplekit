# Utility Functions

The `utility` module collects small helper functions and classes used across the
SimpleKit codebase. These utilities are exported from `src/utility/index.ts` and
can be imported individually by applications.

## Text Measurement

### `measureText(text: string, font: string)`

Returns the rendered width and height of a string using the provided CSS font
specification. Internally an off-screen canvas element is created on first use
to call `CanvasRenderingContext2D.measureText()`. The function returns an object
with `width` and `height` properties or `null` if measurement fails.

```ts
import { measureText } from "simplekit/src/utility";
const size = measureText("Hello", "12pt sans-serif");
```

## Geometry Helpers

### `insideHitTestRectangle(mx, my, x, y, w, h)`

Checks whether a point `(mx, my)` lies within a rectangle. Primarily used by
widgets to perform hit tests on their content boxes.

### `edgeHitTestRectangle(mx, my, x, y, w, h, strokeWidth)`

Determines if a point is on the border of a rectangle with the given stroke
width. This is useful for detecting clicks on outlines.

### `distance(x1, y1, x2, y2)`

Returns the Euclidean distance between two points. Used by various event
translators to detect movement thresholds.

### `random(a, b?)` and `randomInt(a, b?)`

Return floating point or integer random numbers within the given range. If only
`a` is supplied the range is `[0, a)`.

## Vector and Point Classes

Two small classes represent 2D mathematical constructs.

### `Point2`

Represents an immutable point with `x` and `y` coordinates. Points can be added
to a `Vector2` or subtracted from another `Point2` to obtain a vector.

### `Vector2`

Represents a 2D vector with a range of operations:

- `normalize()` – returns a unit vector in the same direction.
- `magnitude()` – length of the vector.
- `dot(other)` – dot product with another vector.
- `cross(other)` – 2D cross product (signed area).
- `multiply(scalar)` – scales the vector.
- `add(other)` / `subtract(other)` – vector arithmetic.

Helper functions `point(x, y)` and `vector(x, y)` create instances of these
classes.

## BoundingBox Class

Although not currently re-exported, `BoundingBox` provides a simple rectangle
object with `pointInside(px, py)` and convenience getters for each edge. It is
used internally for hit testing by some widgets and can serve as a reference for
more advanced geometry tools.
