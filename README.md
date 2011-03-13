# [Katamari Hack](http://kathack.com/ "Katamari Hack")

## What is this?

This is a "bookmarklet" that turns any page into Katamari Damacy.

This was the winner of the 2011 Yahoo HackU contest at University of Washington.

## How does it work?

Short version: css transforms (for things stuck to the katamari), canvas (drawing the katamari), and z-index (illusion of depth).

Long version: The bookmarklet loads jQuery and kh.js into the current page. jQuery is used mostly for .offset() and .css().  kh.js is where all the action happens:

* Splits all the text on the page into words/spans. (StickyNodes::addWords)
* Builds a grid data structure so that intersections with elements can be found quickly (StickyNodes::finalize). Essentially grid[floor(x / 100)][floor(y / 100)] is a list of elements in a 100x100 pixel block. This should probably be an R-tree, but the hot-spot in this program is definitely in the rendering.
* The ball and stripes are drawn in a canvas that gets moved around the page (i.e. position: absolute; left: x; top: y;). See PlayerBall::drawBall.
* When an element is attached to the katamari, a clone is made. The original element is hidden. The new element is moved around by setting -webkit-transform. The transform rotates the element about the rolling axis of the katamari and scales the element to make it look like it's coming out of the page. See PlayerBall::drawAttached, transform_test.html, and transform_test2.html.

