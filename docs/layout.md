# Layout System

SimpleKit uses a flexible yet minimal layout engine for arranging widgets inside
`SKContainer` objects. Layout is always performed in two passes:

1. **Measurement** – Each widget computes its _intrinsic_ size based on its
   content and optional width/height hints.
2. **Layout** – Widgets are positioned and given a final size within their
   parent container.

Every container owns a `LayoutMethod` implementation responsible for measuring
and laying out its child elements. The default is `FixedLayout`, but containers
can opt in to any layout method or even a custom implementation.

A layout method exposes two functions:

```ts
interface LayoutMethod {
  measure(elements: SKElement[]): Size;
  layout(width: number, height: number, elements: SKElement[]): Size;
}
```

The `measure` step returns the minimum size needed to fit the supplied elements
while `layout` positions the elements within the given width and height and
returns the space actually used.

## Measurement and Intrinsic Size

All widgets derive from `SKElement` which tracks `intrinsicWidth` and
`intrinsicHeight`. A widget calls `measure()` on itself and its children to
update these values. Intrinsic size is calculated from explicit width/height
properties, the size of child widgets, or content such as text. Margins and
padding are included so the intrinsic size represents the minimum outer bounds
of the element.

Containers delegate measurement of children to their `LayoutMethod`. After the
layout method computes content size, it sets `contentWidth` and `contentHeight`
on the container so the container's own `measure()` call can account for
padding, margins and optional explicit width/height hints.

## Layout Pass

`layout(width, height)` is called after measurement. The layout method is
responsible for positioning and sizing children within the container's content
box. The container then lays out its own box model using the supplied width and
height.

A few layout methods are provided out of the box.

### FixedLayout

`FixedLayout` simply respects the `x` and `y` position of each child. During
measurement it returns bounds large enough to contain every element at its
specified position. The layout phase calls `element.layout(element.intrinsicWidth,
 element.intrinsicHeight)` on each child and checks if any child overflows the
container when `Settings.layoutWarnings` is enabled.

### CentredLayout

`CentredLayout` stacks all children on top of each other and centres them inside
the container. The measurement phase computes the width and height of the
largest element. During layout each element is optionally stretched if
`fillWidth` or `fillHeight` is set and then translated so that it is centred.
The layout method returns the space actually occupied by the children.

### FillRowLayout

`FillRowLayout` arranges children horizontally in a single row. Children that
set `fillWidth > 0` divide the remaining horizontal space proportionally by
their `fillWidth` values. Fixed width children keep their measured width. The
`gap` constructor option inserts spacing between children. Measurement sums all
intrinsic widths and gaps to determine the minimum container width and uses the
height of the tallest element. During layout the method sets the position of
each child left-to-right and distributes space to any fill elements. The returned
size spans from the first to the last laid out element.

### WrapRowLayout

`WrapRowLayout` is similar to `FillRowLayout` but wraps elements to additional
rows when they no longer fit in the available width. The `gap` option specifies
spacing between elements and rows. Measurement returns the width and height of
the largest single element so containers can be shrunk smaller than the total
wrapped width. Layout iterates over the children, laying them out at their
intrinsic size and wrapping to a new row when necessary. If
`Settings.layoutWarnings` is enabled a warning is logged when rows overflow the
container height.

### Creating Custom Layouts

Any object that satisfies the `LayoutMethod` interface can be assigned to a
container's `layoutMethod` property. Custom layouts can therefore implement
specialised behaviour such as grids or absolute positioning.

```ts
const container = new SKContainer({ layoutMethod: new GridLayout() });
```

### Forcing Layout

Widgets invalidate their layout whenever properties that affect size or position
change. The imperative mode runtime batches these invalidations and performs a
layout pass on the next animation frame. Applications can also call
`invalidateLayout()` from imperative mode to explicitly request a layout pass.

Layout debugging is available via `Settings.debugLayout` which logs detailed
steps of the measurement and layout procedures to the console.
