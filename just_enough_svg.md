## Just Enough SVG

You could easily be forgiven for thinking that HTML -- the HyperText Markup Language -- is the only document format that matters on the web. It is certainly the most prevalent format, by far. In fact, however, most modern browsers can interpret a variety of document formats. One such format particularly relevant for D3.js is Scalable Vector Graphics, or SVG.

SVG is like Adobe Illustrator for the browser. It tells the browser how to draw points, lines, circles, rectangles, and arbitrary shapes and paths, specifying characteristics such as stroke style, width, and color, fill patterns and color, and grouping of multiple elements. Because SVG relies on vectors rather than pixels, browsers can scale it's graphics to any size or precision without distortion.

All modern browsers, including many mobile browsers, support SVG. Of course, the degree of SVG support differs somewhat among the various browsers. Fortunately, however, D3.js only requires minimal SVG functionality to work its magic.

In most cases, you won't have to worry too much about the details of even the simplest SVG, as D3.js will handle it for you. A basic knowledge of SVG can come in handy, however, when you need to figure out why your visualization isn't working like you expect,

### The `<svg>` Element

Although it is possible to construct standalone SVG documents, with D3.js we're still going to limit ourselves to HTML documents, Within those documents, however, we can embed SVG content. That's the purpose of the `<svg>` element. It acts as a container for individual SVG drawing instructions.

Here's an example of embedding SVG within a web page. As in the example, browsers do require explicit width and height attributes for the `<svg>` element. Some browsers accept sizes in units other than pixels, but pixels are universal. (And there is no px suffix.) As we'll discuss below, CSS can be used to style some aspects of SVG. It cannot, however, reliably set the size of the `<svg>` element; you have to use explicit attributes.

```html
<!DOCTYPE html>
<body> 
    <svg width="150" height="100">
        <rect width="50" height="100" x="0" fill="#444" />
        <rect width="50" height="100" x="50" fill="#888" />
        <rect width="50" height="100" x="100" fill="#ccc" />
    </svg>
</body>
</html>
```

One critical aspect of the `<svg>` element is the coordinate system. In SVG, the point (0,0) is in the _upper_ right corner. Increasing y values move the points down the page. This direction is opposite that of traditional mathematical graphs and charts. D3.js can account for it automatically in its scales, but it's worth keeping in mind when you're trying to debug a D3.js graph.

### SVG Primitive Elements

The SVG standard defines several dozen primitive elements. Fortunately, to use D3.js we only need a handful. Here are the key primitives.

* `<circle>` A circle centered at a specified position within the `<svg>` container with a specified radius.
* `<ellipse>` An ellipse centered at a specified position within the `<svg>` container with specified x and y radii.
* `<line>` A line drawn between two points, each specified by x and y coordinates.
* `<path>` A generic shape defined by a series of points. The series of points are contained in the element's `d` attribute using abbreviations specific to SVG.
* `<polygon>` A closed polygon created by connecting a series of points specified by their x and y coordinates.
* `<polyline>` A series of straight line segments connecting a series of points specified by their x and y coordinates.
* `<rect>` A rectangle of specified height and width with its upper left corner specified by x and y coordinates. Corner radii may also be specified for a rounded rectangle.
* `<text>` No surprise here; text at a specified position.
* `<textpath>` Text that follows a path.

### Adjusting SVG Graphics

In addition to its primitive shapes, SVG supports several ways to adjust those shapes and shape combinations. Some of those adjustments can be quite sophisticated and rival the types of operations available in applications such as Adobe's PhotoShop or Illustrator. D3.js, for the most part, sticks with the simple ones like those below.

* `<clippath>` Defines a region within the `<svg>` element. When applied to another element (such as a path), parts of the second element that lie outside of the clipping path are ignored; only those parts within the clipping path are drawn.
* `<g>` A grouping of multiple child elements. (If you've ever "grouped" elements in a drawing program such as Visio or Omnigraffle, it's the same idea.)

### Styling SVG with CSS

In general, SVG relies on explicit attributes to define the elements it draws. There are a few CSS styles, however, that most browsers honor. You can add class and id attributes to SVG elements to use as selectors, or you can use the SVG elements themselves as CSS selectors. The CSS properties that SVG supports include:

* The display property, allowing you to show or hide SVG elements just as you would HTML elements.
* Fill properties, including color, fill-opacity, etc.
* Most of the font properties, including font-family, font-variant, font-weight, font-style, etc.
* Stroke properties, including color, stroke-width, etc.

