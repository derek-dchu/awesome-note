# Vector Graphics
They are geometrical primitives (shapes, points, lines, and polygons) that are all based on vectors to represent images.

In HTML5, we can use both canvas and svg to create vector graphics.

## Canvas vs SVG
### SVG
SVG is used to describe Scalable Vector Graphics, a retained mode graphics model that persist in an in-memory model that can be manipulated through code results in re-rendering.

It has both attributes and presentation attributes.

``` html
<svg>
	<!-- set of elements -->
	<!-- draw a rectangle -->
	<rect id="myRect" height="100px", width="100px">
</svg>
```

### Canvas
*  Immediate mode graphic. 
*  Directly rendering and no save. 
*  Need to re-invoke all drawing commands describe the entire scene each time a new frame is required.
*  Implementation: HTML + JavaScript

    ``` html
    <canvas id="myCanvas"></canvas>
    ```
    +
    
    ``` javascript
    var canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");
    
    // Draw a rectangle
    ctx.fillRect(10, 10, 100, 100);
    ```
    
    Path: we need to call the APIs for each line segment.
        
    ``` javascript
    ctx.beginPath()
    ctx.moveTo(x0, y0);
    ctx.lineTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.closePath();
    ```

*  Limited events and abilities to capture where the mouse is on the image.
*  Does not support scalability.

### Scenarios
*  Performance with large size of screen : Canvas
*  Performance with large number of objects : Svg
*  High Fidelity Complex Vector Documents: Svg
*  Static Images: Svg
*  High Performance: Canvas
*  Complex scenes, real time mathematical animations (with no or few interation): Canvas
*  Pixel manipulation (e.g. video)

Reference: [SVG vs canvas: how to choose](http://msdn.microsoft.com/en-us/library/ie/gg193983.aspx)