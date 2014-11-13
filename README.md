iosViews
========

Sketch plugin to generate ios view code

###export_as_cgContext.sketchplugin
exports your views as CGContext as swift code
so you'll have something like
  CGContextMoveToPoint(...)
  CGContextAddCurveToPoint(...)
  ...

####Why?
It's very annoying to draw in ios in code. But it's annoying to draw in sketch and export it as a picture because you can no longer use the CGRect dimensions from ios. You're stuck with whatever dimensions you picked in sketch.
This will generate a bunch of swift functions, with 1 main function which takes in a CGRect so you can put it in drawRect. All your shapes will be relative to how you depict them in sketch and the CGRect.
For instance, if in sketch you offset your creation 20px from the left, and your arboard is 200px in width. Then at runtime of the ios application your creation will be offset 20*(width/200) where width is the width of the ios CGRect at runtime. This of course works with y offset, width, and height.
When you run the plugin it puts the generated code into your artboard, so if you have a very complex shape (or groups of shapes) pasting the code might be slow or even crash sketch. If it is complex and hasn't crashed, it might take minutes (spinning beachball) but it will work. 
Once I figure out how to make sketch save it to a file and NOT crash sketch, I'll do that and it should work much better.

####How does it work?
- create an artboard (let's say iphone sized)
- create some shape you want that you'd have to do in code otherwise
- select the artboard (make sure it's only the artboard, this is very important)
- run the export_as_cgContext plugin
  - you will get code where everything is relative to the selection (artboard in this case)
  - for each group in the selection, the plugin will generate a swift function named after the group (make sure these are unique)
  - NOTE: for complex shapes the export might be slow (take minutes)

####Unfortunatelly this doesn't work with:
- boolean operations (union, subtract, intersect, difference)
- radius on object (set the radius on individual path instead of object... click enter > highlight points > set radius)
- opacity on object (set your opacity on the color directly, and it will work)
- any color that isn't a solid (gradients, fills don't work)
- more than 1 border or fill (only the last border and last fill are set) 
- blending (will always be treated as "normal")
- shadows, inner shadows, blurs, reflections (will be ignored)
