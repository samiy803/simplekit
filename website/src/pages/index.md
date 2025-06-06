All docs are generated by ChatGPT and could be very, very wrong. If you want to correct them, [open a PR here](https://github.com/samiy803/simplekit).

# Table of Contents
- [Getting Started](/getting-started)
  - [1. Install from npm](/getting-started#1-installing-the-npm-package)
  - [2. Using `npm link`](/getting-started#2-using-npm-link)
  - [3. Adding as a git submodule](/getting-started#3-adding-as-a-git-submodule)
  - [Building SimpleKit](getting-started#building-simplekit)
- [Architecture](/architecture)
  - [Windowing System](/architecture#windowing-system)
  - [Run Loop](/architecture#run-loop)
  - [Event System](/architecture#event-system)
  - [Canvas Mode](/architecture#canvas-mode)
  - [Imperative Mode](/architecture#imperative-mode)
  - [Widgets and Layout](/architecture#widgets-and-layout)
  - [Utility Helpers](/architecture#utility-helpers)
  - [Extending SimpleKit](/architecture#extending-simplekit)
  - [See Also](/architecture#see-also)
- [Canvas Mode](/canvas-mode)
  - [Starting the toolkit](/canvas-mode#starting-the-toolkit)
  - [Drawing and animation](/canvas-mode#drawing-and-animation)
  - [Receiving events](/canvas-mode#receiving-events)
    - [Event translators](/canvas-mode#event-translators)
  - [Typical flow](/canvas-mode#typical-flow)
  - [Customising the Canvas](/canvas-mode#customising-the-canvas)
  - [Graphics Context Methods](/canvas-mode#graphics-context-methods)
-  [Event System](/events)
  - [Fundamental Events](/events#fundamental-events)
  - [SKEvent Hierarchy](/events#skevent-hierarchy)
  - [Event Translators](/events#event-translators)
    - [Writing a translator](/events#writing-a-translator)
  - [Dispatching Events](/events#dispatching-events)
  - [Requesting Focus](/events#requesting-focus)
   - [Custom Events](/events#custom-events)
 - [Imperative UI Mode](/imperative-mode)
   - [Bootstrapping SimpleKit](/imperative-mode#bootstrapping-simplekit)
   - [Building a widget tree](/imperative-mode#building-a-widget-tree)
   - [Events and focus](/imperative-mode#events-and-focus)
   - [Animation and drawing](/imperative-mode#animation-and-drawing)
 - [Layout System](/layout)
   - [Measurement and Intrinsic Size](/layout#measurement-and-intrinsic-size)
   - [Layout Pass](/layout#layout-pass)
     - [FixedLayout](/layout#fixedlayout)
     - [CentredLayout](/layout#centredlayout)
     - [FillRowLayout](/layout#fillrowlayout)
     - [WrapRowLayout](/layout#wraprowlayout)
     - [Creating Custom Layouts](/layout#creating-custom-layouts)
     - [Forcing Layout](/layout#forcing-layout)
 - [Utility Functions](/utility)
   - [Text Measurement](/utility#text-measurement)
     - [`measureText(text: string, font: string)`](/utility#measuretexttext-string-font-string)
   - [Geometry Helpers](/utility#geometry-helpers)
     - [`insideHitTestRectangle(mx, my, x, y, w, h)`](/utility#insidehittestrectanglemx-my-x-y-w-h)
     - [`edgeHitTestRectangle(mx, my, x, y, w, h, strokeWidth)`](/utility#edgehittestrectanglemx-my-x-y-w-h-strokewidth)
     - [`distance(x1, y1, x2, y2)`](/utility#distancex1-y1-x2-y2)
     - [`random(a, b?)` and `randomInt(a, b?)`](/utility#randoma-b-and-randominta-b)
   - [Vector and Point Classes](/utility#vector-and-point-classes)
     - [`Point2`](/utility#point2)
     - [`Vector2`](/utility#vector2)
   - [BoundingBox Class](/utility#boundingbox-class)
