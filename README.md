# sass-base
This sass-base is to be downloaded to each new project.
Usage example of Mixins and functions are described in the Readme file.
## SASS code syntax
SASS syntax is *indented SASS* syntax to reduce clutter in SASS-files. As the indented syntax is harder to find, as usage mostly is bracketed SCSS-coding style, this project will be public and with quite detailed instructions.
# To-do's:
## Basics
- [x] Set up basic sass-files
- [x] Set up example site
- [x] Add media mixin to readme
- [ ] Add basic html structure on example site. Consider if excluded in this project or not...

## Details
- [x] Add touch-action color to links
  
# Functions
## REM conversion
This function takes a pixelvalue (unprefixed, do not use 'px'), divides it with the $base value (set in _variables_default.sass) and returns a rem value. This is the most used function and mixin.
It's not a big thing to change to em, px or percentages in your sass if needed, just ignore this function and modify the other things =)
### The function code
```Sass
@function REM($value)
  @return ($value / $base) + rem
  ```
### SASS usage
```Sass
width: REM(20)
```
### Returns CSS
```CSS
width: 1.25rem;
```

## RGBA helper
This function simplifies the rgba-writing. Enter the Hexadecimal value of the color and the desired opacity value from 0 to 1.
### The function code
```Sass
@function RGBA($color, $opacity)
  @return (rgba($color, $opacity))
```
### SASS usage
```Sass
background: RGBA(#000, 0.9)
```
### Returns CSS
```CSS
background: rgba(0, 0, 0, 0.9);
```

# Mixins
## Calculate width
### The mixin code
```Sass
=calcwidth($calcpercentage, $calcminus)
  width: calc(#{$calcpercentage} - #{$calcminus})
```
### SASS usage
```Sass
+calcwidth(30%, REM($gutter))
```
### Returns CSS
```CSS
width: calc(30% - 1.25rem);
```
## Vendor prefix
Simplifies almost all the vendor prefixes in one line. Work for most cases, except for background-gradients as they are far more complex.
### The mixin code
```Sass
=vendor-prefix($vendorname, $vendorvalue)
  -ms-#{$vendorname}: #{$vendorvalue}
  -o-#{$vendorname}: #{$vendorvalue}
  -moz-#{$vendorname}: #{$vendorvalue}
  -webkit-#{$vendorname}: #{$vendorvalue}
  #{$vendorname}: #{$vendorvalue}
```
### SASS usage
```Sass
+vendor-prefix("box-sizing", "border-box")
```
### Returns CSS
```CSS
-ms-box-sizing: border-box;
-o-box-sizing: border-box;
-moz-box-sizing: border-box;
-webkit-box-sizing: border-box;
box-sizing: border-box; 
```
## Line (border)
Returns a border with the desired width of each border in a shorthand property split on border: and border-width:. 
You do need to set all values or the shorthand will not work. Solid is the default value, so if you want as example dashed you need to add that property to the end.
### The mixin code
```Sass
=line-border($color, $top: 0, $right: 0, $bottom: 2, $left: 0, $style: solid)
  border: $color $style
    width: REM($top) REM($right) REM($bottom) REM($left)
```
### SASS usage
```Sass
+line-border($border-color, 0, 0, 2, 0)
//
+line-border($border-color, 0, 0, 2, 0, dashed)
```
### Returns CSS
```CSS
border: #EBEBEB solid;
border-width: 0rem 0rem 0.125rem 0rem;
//
border: #000 dashed;
border-width: 0.0625rem 0rem 0rem 0.0625rem; 
```
## Span width
Returns a calculated width for the base grid, has special if and else to catch 33% widths.
### The mixin code
```Sass
=spanwidth($width, $margin: $gutter, $percent: "%")
  @if $width == 33
    $width: 33.33333333333334
  @else if $width == 66
    $width: 66.666666666666667
  +calcwidth(#{$width}#{$percent}, REM($margin))
```
### SASS usage
```Sass
+spanwidth(60)
```
### Returns CSS
```CSS
width: calc(60% - 1.25rem);
```
## Media Query
Handles Media Query with an easier approach =)
The mixin takes three arguments and checks if they are applied in the sass-code. You can set each value to false but you do need to set each value in front. See examples below!
### The mixin code
```Sass
=media-query($maxwidth: false, $minwidth: false, $orientation: false)
  @if $maxwidth and $minwidth and $orientation
    @media screen and (max-width: $maxwidth) and (min-width: $minwidth) and (orientation: $orientation)
      @content
  @else if ($maxwidth) and ($minwidth) and ($orientation == false)
    @media screen and (max-width: $maxwidth) and (min-width: $minwidth)
      @content

  @else if ($maxwidth) and ($orientation) and ($minwidth == false)
    @media screen and (max-width: $maxwidth) and (orientation: $orientation)
      @content

  @else if ($minwidth) and ($orientation) and ($maxwidth == false)
    @media screen and (min-width: $minwidth) and (orientation: $orientation)
      @content

  @else if ($maxwidth) and ($minwidth == false) and ($orientation == false)
    @media screen and (max-width: $maxwidth)
      @content

  @else if ($minwidth) and ($orientation == false) and ($maxwidth == false)
    @media screen and (min-width: $minwidth)
      @content

  @else if ($orientation) and ($minwidth == false) and ($maxwidth == false)
    @media screen and (orientation: $orientation)
      @content
```
### SASS usage
```Sass
.span33
  +spanwidth(33)
  +media-query($maxwidth: $tablet-large, $minwidth: $mobile-medium +1) // You do not need to be specific with "$maxwidth:", I just find it easier to read than following examples.
    width: 50%
  +media-query(false, false, landscape) // To set Orientation set the first two as false
    width: 55%
  +media-query(1000px) // To set max-width only: Set this value only, Sass ignores the others if no value set (as default value false kicks in)
    width: 60%
  +media-query(false, 885px) // To set min-width only: Set max-width to false.
    width: 65%
```
### Returns CSS
```CSS
.span33 {
  width: calc(33.33333% - 1.25rem); }
  @media screen and (max-width: 880px) and (min-width: 451px) {
    .span33 {
      width: 50%; } }
  @media screen and (orientation: landscape) {
    .span33 {
      width: 55%;  } }
  @media screen and (max-width: 1000px) {
    .span33 {
      width: 60%;  } }
  @media screen and (min-width: 885px) {
    .span33 {
      width: 65%;  } }
```