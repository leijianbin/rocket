# Rocket 2.0

Rocket is a powerful SASS library to help web developers handle layout, color and other components.    
[demos](http://creatiointl.org/gallery/william/rocket/layout-grid.php)   
[Changelog](https://github.com/ganlanyuan/rocket/blob/master/changelog.md)   
[sassdoc](http://creatiointl.org/gallery/william/sassdoc/)   

# Install

Download [rocket](https://github.com/ganlanyuan/rocket.git) or install with [Bower](http://bower.io/): 
```` bash
$ bower install rocket
````
Or install with [git](http://www.git-scm.com/):
```` bash
$ git clone https://github.com/ganlanyuan/rocket.git
````

# Structure

```` html      
addons
  |opacity
  |ie-rgba
  |rems
  |breakpoint
  |visible
  |hidden
  |type

layout
  |container
  |wrap
  |span
  |span-calc
  |two-columns
  |gallery
  |justify
  |center

components
  |button
  |media
  |off-canvas
  |dropdown
  |tooltip
  |flex-video

slider
  |gallery
  |carousel

color functions
  |analogous
  |contrast
  |adjacent
  |complementary
  |split-complementary
  |triad
  |rectangle
  |square
````

#【 Layout 】
#### layout setting
```` scss
// default setting
// items: 5;
// perpage: 2;
// gutter: 10px; (default)
// slide-by-page: true;
// default: true; (default styles for controls and dots)
$layout: (
  container: 1024px,
  columns: 12,
  gutter: 2%,
);
````

#### container
The container of the main content. It can be center, left or right aligned.
```` scss
@include container {$key}
// pattern
$key: ($container $gutter) $align;

.wrapper { @include container(1200px); }
// container: 1200px;
// gutter: 2%; (default)
// align: center; (default)

.wrapper { @include container(1200px 20px left); }
// container: 1200px
// gutter: 20px;
// align: left;

.wrapper { @include container(64em 2% center); }
// container: 64em;
// gutter: 2%;
// align: center;
````

#### wrap
Grid wrapper, works with `span` when using a fixed value for `gutter`.
```` scss
@include wrap($key);
// pattern
$key: $gutter;

.wrapper { @include wrap(20px); }
// gutter: 20px;
````

#### span
`span` is used to create grid. You can use fixed gutter (px, em, rem) or flexible gutter (%). If you use fixed gutter, you need set the parent element as a `wrap`, or you can use `span-calc`.
```` scss
@include span($key);
// pattern
$key: ($column at $location of $columns) $gutter (move $move) (float $float) last keep;

.nav { @include span(3); }
// column: 3;
// columns: 12; (default)
// columns: 2%; (default)

.nav { @include span(11 of 16 2%); }
// column: 3;
// columns: 12;
// gutter: 2%;

.nav { @include span(11 at 5 of 16 2%); }
// location: 5; (isolate mode)

.nav { @include span(11 of 16 2% right move -5 last); }
// last: true; (The last column)
// float: right;
// move: -5; (move left 5 columns)
````
*Isolate mode*: read [this article](http://www.palantir.net/blog/responsive-design-s-dirty-little-secret) for more detail. If you want to use isolate mode on one column, other siblings columns also need use isolate mode: 
````scss
.col-1 { @include span(.. at ..); } 
.col-2 { @include span(.. at ..); } 
````
*Keep*: you may want keep some parts(float, gutter) constant when doing media query, then you can use `keep`;
````scss
// scss
.nav { @include span(7 of 11 2%); }

@media screen and (min-width: 800px) {
  .nav { @include span(4 of 11 keep); }
}
````
````css
/* css */
.nav {
  float: left;
  width: 62.9090909091%;
  margin-right: 2%;
}

@media screen and (min-width: 800px) {
  .nav { width: 35.0909090909%; }
}
````
*Nested grid*: use function `span` to calculate the right gutter. We're still working on this to make it even easier.
```` html
<!-- nested grid -->
<div class="parent">
  <div class="child-1"></div>
  <div class="child-2"></div>
</div>
<div class="aside"></div>
````
```` scss
$gutter-1: 2%;
$gutter-2: ($gutter-1 / span(7 of 10 2%)); 
// or you can $gutter-2: ($gutter-1 / 69.99%)

.parent {
  @include span(7 of 10 $gutter-1);
  .child-1 { @include span(9 of 16 $gutter-2); }
  .child-2 { @include span(7 of 16 $gutter-2); }
}
.aside { @include span(3 of 10 $gutter-1); }
````

#### span-calc
`span-calc` is using `css-calc` to create columns, old browser (e.g. IE8) will not be supported.
```` scss
@include span-calc($key);
// pattern
$key: ($column of $columns) $gutter (move $move) (float $float) last keep;

.nav { @include span-calc(3); }
// column: 3;
// columns: 12; (default)
// columns: 20px; (default)

.nav { @include span-calc(11 of 16 30px); }
// column: 3;
// columns: 12;
// gutter: 2%;

.nav { @include span(last right 11 of 16 30px move -5); }
// last: true; (The last column)
// float: right;
// move: -5; (move left 5 columns)

// Keep: similar with span(keep).
// Tips: gutter must be a fixed value(px, em, rem).
````

[Grid demos](http://creatiointl.org/gallery/william/rocket/layout-grid.php)

#### gallery
`gallery` is for creating picture galleries.
```` scss
@include gallery($key);
//pattern
$key: $per-row $gutter (child $child) $float $position keep;

.pic { @include gallery(3 2% child li middle); }
// per-row: 3;
// gutter: 2%;
// child: li;
// float: left; (default)
// position: middle; (same padding on the top and bottom for each item)

@media screen and (min-width: 768px) {
  .pic { @include gallery(4 child li keep); }
}
// Keep: similar with span(keep).
````
[demo](http://creatiointl.org/gallery/william/rocket/layout-gallery.php)

#### justify
`justify` is for creating `justify` list.
```` scss
@include justify-flex();

.no-flexbox {
  @include justify(div);
  // for old browsers
  // child: div
}
````
[demo](http://creatiointl.org/gallery/william/rocket/layout-justify.php)

#### center
`center` is for creating both horizontal and vertical center aligned layout.
```` scss
@include center($key);
// pattern
$key: $child $align;

.banner { @include center(div left); }
// child: div;
// align: left; (left | right | center, for old browser)
````
[demo](http://creatiointl.org/gallery/william/rocket/layout-center.php)

#### two-columns
`two-columns` is for creating a two columns layout. One of them has a fixed width.
````html
<!-- Add "data-col-main", "data-col-aside" attributes to your markup. -->

<div class="wrapper">

  <!-- main -->
  <div data-col-main=""></div>

  <!-- aside -->
  <div data-col-aside=""></div>
</div>
````
```` scss
// scss
@include two-columns($key);
// pattern
$key: $direction $aside-width (gutter $gutter);

.wrapper { @include two-columns(left 300px gutter 30px); }
// direction: left; (aside is on the left)
// aside-width: 300px;
// gutter: 30px;
````
[demo](http://creatiointl.org/gallery/william/rocket/layout-two-columns.php)


#【 Components 】
#### button
````scss
@include button($key);
// pattern
$key: $font-size $padding $background-color radius round hover;

.button { @include button(14px #00c8ff '0.8em 1em' radius hover); }
// font-size: 14px;
// background-color: #00c8ff;
// padding: 0.8em 1em; (Tips: padding must be quoted)
// radius: 0.22em; (default, you can modify it by change the varible "$button-radius: 0.22em;" )
// hover: true; (change background-color when mouse over)
````
[demo](http://creatiointl.org/gallery/william/rocket/components-button.php)

#### Media list
`media` displays a media object (images, video, audio) to the left or right of a block.
```` scss
@include media($key);
// pattern
$key: $gutter (child $child1 $child2 ...);

.news { @include media(); } 
// gutter: 10px; (default)
// child: '*'; (default)

.news-right { @include media(1em child '.media' '.media-body'); } 
// gutter: 1em;
// child: '.media' '.media-body';
````
[demo](http://creatiointl.org/gallery/william/rocket/components-media-list.php)

#### flex video
```` scss
@include flex-video($key);
// pattern
$key: $ratio (child $child);

.flex-video { @include flex-video(3/4); }
// ratio: 3/4;
// child: iframe, object, embed; (default)

.flex-video { @include flex-video(9/16 embed); }
// ratio: 9/16;
// child: embed; 
````
[demo](http://creatiointl.org/gallery/william/rocket/components-flex-video.php)

#### dropdown
```` html
<!-- 
  if you set $show: click, add these markup to your dropdown 
  make sure the value of "for" match the checkbox id
  -->

<input type="checkbox" name="" id="dropdown">
<label for="dropdown">▼</label>
````
```` scss
@include dropdown($key);
// pattern
$key: $child $show $style default;

.dropdown { @include dropdown(ul hover display default); }
// child: ul;
// show: hover; (hover | click)
// style: display; (scale | display)
// default: true; (use default dropdown menu style)
````
[demo](http://creatiointl.org/gallery/william/rocket/components-dropdown.php)

#### tooltip
pure css `tooltip`
```` scss
@include tooltip($key);
// pattern
$key: $direction $color radius (width $width) (height $height);

.tooltip { @include tooltip(radius right #b02df3 width 300px); }
// radius: 0.22em; (This can be custmized by changing "$tooltip-radius: 0.22em !default;")
// direction: right;
// background-color: #b02df3;
// width: 300px; (for old browsers)

// To make tooltip shows perfect, 
// it's better to set tooltip box as a block element, 
// or give it a width;
````
[demo](http://creatiointl.org/gallery/william/rocket/components-tooltip.php)

#### off-canvas
`off-canvas` is for creating the navigation of mobile site.
````html
<!-- include kit.js -->
<script src="path/to/kit.min.js"></script>

<!-- off-canvas -->
<nav class="nav">
  <ul>
    <li><span data-offcanvas-close>close</span></li>
    <li><a href="">item01</a></li>
    <li><a href="">item02</a></li>
    <li><span data-icon-haschild><span class="ic-angle-right"></span></span><a href="">item03</a>
      <ul data-offcanvas-subnav>
        <li data-offcanvas-back>back</li>
        <li><a href="">sub item02</a></li>
        <li><a href="">sub item03</a></li>
        <li><a href="">sub item04</a></li>
      </ul>
    </li>
    <li><a href="">item04</a></li>
  </ul>
</nav>

<!-- page -->
<div class="page">
  <!-- page cover -->
  <div data-page-cover=""></div>

  <!-- nav icon -->
  <div data-icon-nav></div>
</div>
````
```` scss
// *** off-canvas *** //
@include off-canvas($key);
// pattern
$key: $style $direction animation $off-canvas-width $padding $background-color;

.nav { @include off-canvas(translate 300px '1em' left #102244 animation); }
// style: translate; (move | transition | reveal)
// off-canvas-width: 300px;
// nav-item-padding: 1em;
// direction: left; (left | right)
// off-canvas-background-color: #102244;
// animation: true;

// *** page-container *** //
@include page-container($key);
// pattern
$key: $style $direction $off-canvas-width $cover-bg;

.page { @include page-container(translate left 300px); }
// style: translate;
// off-canvas-width: 300px;
// direction: left;
````
[demo](http://creatiointl.org/gallery/william/rocket/components-off-canvas.php)


#【 Addons 】
#### type
`type` is a shorthand mixin for type.
```` scss
@include type($key);
// pattern
$key: $font-size $font-weight $font-style $line-height $font-family $text-transform; 

h1 { @include type(20px 'Georgia, Helvetica, sans-serif' 1.4 bold italic) }
// font-size: 20px;
// font-weight: bold; 
// font-style: italic; 
// font-family: 'Georgia, Helvetica, sans-serif';
// line-height: 1.4;

// Tips: to set 'font-weight', 'font-style' or 'text-transform' value 
// to 'inherit' or 'normal', 
// you need add prefix 'weight-', 'style-' or 'transform-'.

// $font-weights: thin, hairline, 'extra light', 'ultra light', lighter, light, normal, medium, 'semi bold', 'demi bold', bold, bolder, 'extra bold', black, heavy, 100, 200, 300, 400, 500, 600, 700, 800, 900, weight-normal, weight-inherit !default;
// $font-styles: italic, oblique, style-normal, style-inherit !default;
// $text-transforms: capitalize, uppercase, lowercase, none, full-width, transform-inherit !default;
````
[demo](http://creatiointl.org/gallery/william/rocket/addons-type.php)

#### opacity
`opacity` for old IE.
```` scss
@include opacity($key);
// pattern
$key: $key: $opacity;

.example { @include opacity(0.3); }
````

#### breakpoint
A shorthand @mixin for break point.
```` scss
@include breakpoint($key);
// pattern
$key: $condition $media $breakpoints;

@include breakpoint('min' 640) {};
// output: @media (min-width: 40em) {};
@include breakpoint('max' 640 screen) {};
// output: @media screen and (max-width: 40em) {};
@include breakpoint(640 767) {};
// output: @media (min-width: 40em) and (max-width: 47.94em) {};
````

#### visible
A shorthand @mixin for hide elements on some parts of viewport.
```` scss
@include visible($key);
// pattern
$key: $media $breakpoints;

@include visible(500)
// visible on 500px up on all media
````

#### hidden
A shorthand @mixin for hide elements on some parts of viewport.
```` scss
@include hidden($key);
// pattern
$key: $media $breakpoints;

@include hidden(screen 300 500 700)
// hidden between 300px and 500px, and 700px up on screen
````
[demo](http://creatiointl.org/gallery/william/rocket/addons-visibility.php)


#【 Color Functions 】
Please refer to [Adobe Kuler](https://color.adobe.com/create/color-wheel/) and [paletton](http://paletton.com/#uid=1000u0kllllaFw0g0qFqFg0w0aF).   

#### contrast
Get a contrast `font-color` based on the `background-color`.
```` scss
@include contrast($key);
// pattern
$key: $color (light $light) (dark $dark);

.main { color: contrast(#a6e36e); }
// color: #a6e36e;
// light: #fff; (default)
// dark: #000; (default)
````
[demo](http://creatiointl.org/gallery/william/rocket/color-contrast.php)

#### adjacent
`adjacent` is for creating adjacent colors.
```` scss
@include adjacent($key);
// pattern
$key: $color $order (saturation $saturation) (lightness $lightness) (dist $dist);

.main { color: adjacent(#a6e36e -1 saturation 10% lightness -20% dist 20); }
// color: #a6e36e;
// order: -1;
// saturation: 10%;
// lightness -20%;
// distribution: 20;
````
[demo](http://creatiointl.org/gallery/william/rocket/color-adjacent.php)

#### complementary
`complementary` is for getting a complementary color.
```` scss
@include complementary($key);
// pattern
$key: $color (saturation $saturation) (lightness $lightness) (dist $dist);

.main { color: complementary(#a6e36e saturation 20%); }
// color: #a6e36e;
// saturation: 20%;
````
[demo](http://creatiointl.org/gallery/william/rocket/color-complementary.php)

#### split-complementary
`split-complementary` is for getting split-complementary colors based on a given color.
```` scss
@include split-complementary($key);
// pattern
$key: $color $order (saturation $saturation) (lightness $lightness) (dist $dist);

.main { color: split-complementary(#a6e36e 2); }
// color: #a6e36e;
// order: 2; (-2 | -1 | 1 | 2)
````
[demo](http://creatiointl.org/gallery/william/rocket/color-split-complementary.php)

#### triad
`triad` is for getting triad colors based on a given color.
```` scss
@include triad($key);
// pattern
$key: $color $order (saturation $saturation) (lightness $lightness) (dist $dist);

.main { color: triad(#a6e36e, 2); }
// color: #a6e36e;
// order: 2; (-2 | -1 | 1 | 2)
````
[demo](http://creatiointl.org/gallery/william/rocket/color-triad.php)

#### rectangle
`rectangle` is for getting rectangle colors based on a given color.
```` scss
@include rectangle($key);
// pattern
$key: $color $order (saturation $saturation) (lightness $lightness) (dist $dist);

.main { color: rectangle(#a6e36e, -3); }
// color: #a6e36e;
// order: -3; (-3 | -2 | -1 | 1 | 2 | 3)
````
[demo](http://creatiointl.org/gallery/william/rocket/color-rectangle.php)

#### square
`square` is for getting square colors based on a given color.
```` scss
@include square($key);
// pattern
$key: $color $order (saturation $saturation) (lightness $lightness) (dist $dist);

.main { color: square(#a6e36e, 3); }
// color: #a6e36e;
// order: 3; (-3 | -2 | -1 | 1 | 2 | 3)
````
[demo](http://creatiointl.org/gallery/william/rocket/color-square.php)


#【 Pure CSS slideshow 】
A pure CSS responsive slider with `previous/next` buttons, `nav dots`, `autoplay`(IE8- are not supported), `autoheight` and more. It works well on modern browsers and IE8+, but it doesn't support `loop` and `lazyload` for now.

#### markup
First, set a specific class (or ID) for each slider.   
Then, use this class (or id) to set up the radio names and IDs as well as labels.   
In the example shows on the left, I used banner as my specific class.    
```` html
<div class="banner">
  <input type="radio" name="banner" id="banner-1" checked="">
  <input type="radio" name="banner" id="banner-2">
  <input type="radio" name="banner" id="banner-3">
  <input type="radio" name="banner" id="banner-4">
  <input type="radio" name="banner" id="banner-5">
  <input type="checkbox" name="banner-autoplay" id="banner-autoplay" checked="">
  <div class="outer">
    <ul class="inner">
      <li> slider01 </li>
      <li> slider02 </li>
      <li> slider03 </li>
      <li> slider04 </li>
      <li> slider05 </li>
    </ul>
  </div>
  <div class="controls">
    <label for="banner-1"><span class="prev"></span><span class="next"></span></label>
    <label for="banner-2"><span class="prev"></span><span class="next"></span></label>
    <label for="banner-3"><span class="prev"></span><span class="next"></span></label>
    <label for="banner-4"><span class="prev"></span><span class="next"></span></label>
    <label for="banner-5"><span class="prev"></span><span class="next"></span></label>
  </div>
  <div class="dots">
    <label for="banner-1"><span class="normal"></span><span class="active"></span></label>
    <label for="banner-2"><span class="normal"></span><span class="active"></span></label>
    <label for="banner-3"><span class="normal"></span><span class="active"></span></label>
    <label for="banner-4"><span class="normal"></span><span class="active"></span></label>
    <label for="banner-5"><span class="normal"></span><span class="active"></span></label>
  </div>
  <div class="autoplay"><label for="banner-autoplay"><span></span></label></div>
</div>
````
#### slider-gallery
```` scss
// basic
@include slider-gallery($key);
// pattern
$key: $items $ratio autoplay default;

.slider { @include slider-gallery(5 autoplay default); }
// items: 5;
// ratio: 9/16; (default) 
// autoplay: true;
// default: true; (default styles for controls and dots)

// customise dots and controls
.slider .dots .normal { ... }
.slider .dots .active { ... }
.slider .controls .prev { ... }
.slider .controls .next { ... }

// customise items
.slider {
  .outer { overflow: visible; }
  li { @include transform(scale(1.1)); }
  @for $i from 1 through 5 {
    #gallery-b-#{$i}:checked ~ .slider-container li:nth-child(#{$i}) { @include transform(scale(1)); }
  }
}
````

*Autoheight*       
Add `kit.min.js` to `head`, and then put `autoheight-gallery` attribute to the slideshow container (.outer).
```` html
<!-- include kit.js -->
<script src="path/to/kit.min.js"></script>

<!-- add "autoheight-gallery" attribute -->
<div class="outer" autoheight-gallery></div>
````

[demo](http://creatiointl.org/gallery/william/rocket/slider-gallery.php)

#### slider-carousel
```` scss
// basic
@include slider-carousel($key);
// pattern
$key: ($items by $perpage) $gutter bypage center autoplay default;

.slider { @include slider-carousel(5 by 2 bypage default); }
// items: 5;
// perpage: 2;
// gutter: 10px; (default)
// slide-by-page: true;
// default: true; (default styles for controls and dots)
````

*Autoheight*   
Add `kit.min.js` to `head`, and then put `autoheight-carousel` attribute to the slideshow container (.outer). 
```` html
<!-- include kit.js -->
<script src="path/to/kit.min.js"></script>

<!-- add "autoheight-carousel" attribute -->
<div class="outer" autoheight-carousel></div>
````

[demo](http://creatiointl.org/gallery/william/rocket/slider-carousel.php)


#【 kit.js 】
Kit.js is small Javascript library similar with jQuery. Kit.js works well on IE8 and up, and on other morden browsers.   
The follow metheds are available:   
`on`, `off`, `click`, `mouseover`, `mouseout`, `focus`, `blur`, `submit`, `keydown`, `keyup`,   
`find`, `eq`, `filter`, `first`, `last`, `parent`, `parents`, `children`, `firstChild`, `lastChild`, `siblings`, `prev`, `prevAll`, `next`, `nextAll`,   
`hide`, `show`, `fadeIn`, `remove`,   
`text`, `html`, `attr`, `css`, `addClass`, `removeClass`, `toggleClass`, `hasClass`,   
`outerWidth`, `outerHeight`, `getTop`, `getLeft`, `offset(left top)`,   
`before`, `after`, `append`, `prepend`  

#### Ready
```` javascript
ready(function () {
  ...
});
````

#### Dom methods
```` javascript
k('.header').parent().addClass('newclass');
k('.button').siblings('p').css({
  'color': 'red',
  'line-height': '1.5'
});
````

#### Event
```` javascript
k('.icon-menu').click(function() {
  k(this).parent().toggleClass('active');
});

k('.news').on('mouseover', function() {
  k(this).css('background-color', 'blue');
});
````

#### forEach
```` javascript
k('.site-nav a').forEach(function (el) {
  el.onclick = function () {
    var targetId = k(this).attr('href');
        targetPosition = k(targetId).getTop();
    scrollTo(targetPosition, 400);
    return false;
  }
});
````

#### Reach
`reach` is a function to check if target element reach the edge of browser.  
```` javascript
if (k(el).reach('middle',0)) {
  // if target element reach the middle of the browser, do something
}
if (k(el).reach('top',20)) {
  // if target element reach the point which is under the top of the browser 20px, do something
}
if (k(el).reach('bottom',0)) {
  // if target element reach the bottom of the browser, do something
}
````

#### scrollTo
Scroll to some point in a given period of time.  
```` javascript
scrollTo (to,duration);

k('.icon-menu').click(function() {
  scrollTo (0,200); // scroll to top in 200ms
});
````

#### numIncrease
Increase numbers in given period of time.
```` javascript
numIncrease(element, from, to, duration);

document.onload = function  () {
  numIncrease(k('.follows'), 0, 200000, 400);
};
````

#### animate
```` javascript
animate(el, attr, from, to, duration);
animate(k('.target'), 'left', 0, 20, 400);
````
