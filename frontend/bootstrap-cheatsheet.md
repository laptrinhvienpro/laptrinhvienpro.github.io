# Bootstrap 4.4.x Cheatsheet

```scss
/* -------------------------------------------------------------------------- */
// All Bootstrap 4 Sass Mixins [Cheat sheet]
// Updated to Bootstrap v4.4.x
// @author https://anschaef.de
// @see https://github.com/twbs/bootstrap/tree/master/scss/mixins
/* -------------------------------------------------------------------------- */

// Grid variables
$grid-columns: 12;
$grid-gutter-width: 30px;

$grid-breakpoints: (
xs: 0,
sm: 576px,
md: 768px,
lg: 992px,
xl: 1200px
);

$container-max-widths: (
sm: 540px,
md: 720px,
lg: 960px,
xl: 1140px
);

/* -------------------------------------------------------------------------- */

// Breakpoint viewport sizes and media queries.
// Breakpoints are defined as a map of (name: minimum width), order from small to large:
//    (xs: 0, sm: 576px, md: 768px, lg: 992px, xl: 1200px)
// Media of at least the minimum breakpoint width. No query for the smallest breakpoint.
// Makes the @content apply to the given breakpoint and wider.
@include media-breakpoint-up($name, $breakpoints: $grid-breakpoints)
// Media of at most the maximum breakpoint width. No query for the largest breakpoint.
// Makes the @content apply to the given breakpoint and narrower.
@include media-breakpoint-down($name, $breakpoints: $grid-breakpoints)
// Media that spans multiple breakpoint widths.
// Makes the @content apply between the min and max breakpoints
@include media-breakpoint-between($lower, $upper, $breakpoints: $grid-breakpoints)
// Media between the breakpoint's minimum and maximum widths.
// No minimum for the smallest breakpoint, and no maximum for the largest one.
// Makes the @content apply only to the given breakpoint, not viewports any wider or narrower.
@include media-breakpoint-only($name, $breakpoints: $grid-breakpoints)

// Example usage
.some-class {
    @include media-breakpoint-up(sm) {
        // Larger than sm: 576px
        display: inline;
    }
    @include media-breakpoint-down(md) {
        // Smaller than md: 768px
        display: block;
    }
}

/* -------------------------------------------------------------------------- */

// Grid system
// @see https://getbootstrap.com/docs/4.4/layout/grid/#sass-mixins
// Generate semantic grid columns with these mixins.
@include make-container($gutter: $grid-gutter-width)
// For each breakpoint, define the maximum width of the container in a media query
@include make-container-max-widths($max-widths: $container-max-widths, $breakpoints: $grid-breakpoints)
// Creates a wrapper for a series of columns
@include make-row($gutter: $grid-gutter-width)
// Make the element grid-ready (applying everything but the width)
@include make-col-ready($gutter: $grid-gutter-width)
// Add a `max-width` to ensure content within each column does not blow out
// the width of the column. Applies to IE10+ and Firefox. Chrome and Safari
// do not appear to require this.
@include make-col($size, $columns: $grid-columns)
// Reset earlier grid tiers
@include make-col-auto()
@include make-col-offset($size, $columns: $grid-columns)
// Row columns
// Specify on a parent element(e.g., .row) to force immediate children into NN
// numberof columns. Supports wrapping to new lines, but does not do a Masonry
// style grid.
@include row-cols($count)

// Example usage
/*
<div class="example-container">
  <div class="example-row">
    <div class="example-content-main">Main content</div>
    <div class="example-content-secondary">Secondary content</div>
  </div>
</div>
*/
.example-container {
    width: 800px;
    @include make-container();
}
.example-row {
    @include make-row();
}
.example-content-main {
    @include make-col-ready();

    @include media-breakpoint-up(sm) {
        @include make-col(6);
    }
    @include media-breakpoint-up(lg) {
        @include make-col(8);
    }
}
.example-content-secondary {
    @include make-col-ready();

    @include media-breakpoint-up(sm) {
        @include make-col(6);
    }
    @include media-breakpoint-up(lg) {
        @include make-col(4);
    }
}

/* -------------------------------------------------------------------------- */

// Alerts
@include alert-variant($background, $border, $color)

/* -------------------------------------------------------------------------- */

// Contextual backgrounds
@include bg-variant($parent, $color, $ignore-warning: false)
@include bg-gradient-variant($parent, $color)

/* -------------------------------------------------------------------------- */

// Badges
@include badge-variant($bg)

/* -------------------------------------------------------------------------- */

// Single side border-radius
@include border-radius($radius: $border-radius, $fallback-border-radius: false)
@include border-top-radius($radius)
@include border-right-radius($radius)
@include border-bottom-radius($radius)
@include border-left-radius($radius)
@include border-top-left-radius($radius)
@include border-top-right-radius($radius)
@include border-bottom-right-radius($radius)
@include border-bottom-left-radius($radius)

/* -------------------------------------------------------------------------- */

// Shadows
@include box-shadow($shadow...)

/* -------------------------------------------------------------------------- */

// Button variants
// Easily pump out default styles, as well as :hover, :focus, :active,
// and disabled options for all buttons
@include button-variant($background, $border, $hover-background: darken($background, 7.5%), $hover-border: darken($border, 10%), $active-background: darken($background, 10%), $active-border: darken($border, 12.5%))
@include button-outline-variant($color, $color-hover: color-yiq($color), $active-background: $color, $active-border: $color)
// Button sizes
@include button-size($padding-y, $padding-x, $font-size, $line-height, $border-radius)

/* -------------------------------------------------------------------------- */

// Clearfix
// @see https://getbootstrap.com/docs/4.4/utilities/clearfix/
@include clearfix()

/* -------------------------------------------------------------------------- */

// Float
// @see https://getbootstrap.com/docs/4.4/utilities/float/
@include float-left()
@include float-right()
@include float-none()

/* -------------------------------------------------------------------------- */

// Form control focus state
// Generate a customized focus state and for any input with the specified color,
// which defaults to the `@input-border-color-focus` variable.
@include form-control-focus($ignore-warning: false)
@include form-validation-state-selector($state)
@include form-validation-state($state, $color, $icon)

/* -------------------------------------------------------------------------- */

// Gradients
@include gradient-bg($color)
// Horizontal gradient, from left to right
// Creates two color stops, start and end, by specifying a color and position for each color stop.
@include gradient-x($start-color: $gray-700, $end-color: $gray-800, $start-percent: 0%, $end-percent: 100%)
// Vertical gradient, from top to bottom
// Creates two color stops, start and end, by specifying a color and position for each color stop.
@include gradient-y($start-color: $gray-700, $end-color: $gray-800, $start-percent: 0%, $end-percent: 100%)
@include gradient-directional($start-color: $gray-700, $end-color: $gray-800, $deg: 45deg)
@include gradient-x-three-colors($start-color: $blue, $mid-color: $purple, $color-stop: 50%, $end-color: $red)
@include gradient-y-three-colors($start-color: $blue, $mid-color: $purple, $color-stop: 50%, $end-color: $red)
@include gradient-radial($inner-color: $gray-700, $outer-color: $gray-800)
@include gradient-striped($color: rgba($white, .15), $angle: 45deg)

/* -------------------------------------------------------------------------- */

// Framework grid generation
// Used only by Bootstrap to generate the correct number of grid classes given
// any value of `$grid-columns`.
@include make-grid-columns($columns: $grid-columns, $gutter: $grid-gutter-width, $breakpoints: $grid-breakpoints)

/* -------------------------------------------------------------------------- */

// Hover state
@include hover()
@include hover-focus()
@include plain-hover-focus()
@include hover-focus-active()

/* -------------------------------------------------------------------------- */

// Responsive image
// Keep images from scaling beyond the width of their parents.
@include img-fluid()

// Retina image
// Short retina mixin for setting background-image and -size.
@include img-retina($file-1x, $file-2x, $width-1x, $height-1x)

/* -------------------------------------------------------------------------- */

// List Groups
@include list-group-item-variant($state, $background, $color)

/* -------------------------------------------------------------------------- */

// Lists
// Unstyled keeps list items block level, just removes default browser padding and list-style
@include list-unstyled()

/* -------------------------------------------------------------------------- */

// Horizontal dividers
// Dividers (basically an hr) within dropdowns and nav lists
@include nav-divider($color: $nav-divider-color, $margin-y: $nav-divider-margin-y, $ignore-warning: false)

/* -------------------------------------------------------------------------- */

// Navbar vertical align
// Vertically center elements in the navbar.
// Example: an element has a height of 30px, so write out `.navbar-vertical-align(30px);` to calculate the appropriate top margin.
// @include navbar-vertical-align($element-height)

/* -------------------------------------------------------------------------- */

// Pagination
@include pagination-size($padding-y, $padding-x, $font-size, $line-height, $border-radius)

/* -------------------------------------------------------------------------- */

// Text reset
@include reset-text()

/* -------------------------------------------------------------------------- */

// Resize anything
@include resizable($direction) // Options: horizontal, vertical, both

/* -------------------------------------------------------------------------- */

// Only display content to screen readers
// @see https://getbootstrap.com/docs/4.4/utilities/screen-readers/
@include sr-only()
// Use in conjunction with .sr-only to only display content when it's focused.
// Useful for "Skip to main content" links; see https://www.w3.org/TR/2013/NOTE-WCAG20-TECHS-20130905/G1
@include sr-only-focusable()

/* -------------------------------------------------------------------------- */

// Sizing shortcuts
@include size($width, $height: $width)

/* -------------------------------------------------------------------------- */

// Tables
@include table-row-variant($state, $background)

/* -------------------------------------------------------------------------- */

// Typography
@include text-emphasis-variant($parent, $color)

/* -------------------------------------------------------------------------- */

// CSS image replacement
// @see https://getbootstrap.com/docs/4.4/utilities/image-replacement/
@include text-hide($ignore-warning: false)

/* -------------------------------------------------------------------------- */

// Text truncate
// Requires inline-block or block for proper styling
@include text-truncate()

/* -------------------------------------------------------------------------- */

// Transitions
@include transition($transition...)

/* -------------------------------------------------------------------------- */

// Visibility
// @see https://getbootstrap.com/docs/4.4/utilities/visibility/
@include invisible($visibility) // Options: collapse, hidden, visible

/* -------------------------------------------------------------------------- */

// Carets
@include caret-down()
@include caret-up()
@include caret-right()
@include caret-left()
@include caret($direction: down)

/* -------------------------------------------------------------------------- */

// Deprecate mixin
// This mixin can be used to deprecate mixins or functions.
// `$enable-deprecation-messages` is a global variable, `$ignore-warning` is a variable that can be passed to
// some deprecated mixins to suppress the warning (for example if the mixin is still be used in the current version of Bootstrap)
@include deprecate($name, $deprecate-version, $remove-version, $ignore-warning: false)

/* -------------------------------------------------------------------------- */

// New since v4.3: Responsive font-size mixin
// @see https://getbootstrap.com/docs/4.4/content/typography/#responsive-font-sizes
// Aliases: @include font-size($fs, $important: false), @include responsive-font-size($fs, $important: false)
@include rfs($fs, $important: false)
```