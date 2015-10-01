# sass-base
The base that should be downloaded to each new project

# To-do's:
## Basics
Set up basic sass-files
Set up example site
Add basic html structure on example site. Consider if excluded in this project or not...
Add media mixin

## Details
Add touch-action color to links

# Mixins and functions

## Mixins
### Calculate width
#### The mixin code
=calcwidth($calcpercentage, $calcminus)
  width: calc(#{$calcpercentage} - #{$calcminus})
#### SASS usage
+calcwidth(30%, $gutter)
#### Returns CSS
width: calc(30% - 16px)

### Vendor prefix
Simplify almost all the vendor prefixes in one line. Work for most cases. Does not work for background-gradients as they ar far more complex.
#### The mixin code
=vendor-prefix($vendorname, $vendorvalue)
  -ms-#{$vendorname}: #{$vendorvalue}
  -o-#{$vendorname}: #{$vendorvalue}
  -moz-#{$vendorname}: #{$vendorvalue}
  -webkit-#{$vendorname}: #{$vendorvalue}
  #{$vendorname}: #{$vendorvalue}
#### SASS usage
+vendor-prefix("box-sizing", "border-box")
#### Returns CSS
-ms-box-sizing: border-box;
-o-box-sizing: border-box;
-moz-box-sizing: border-box;
-webkit-box-sizing: border-box;
box-sizing: border-box;
  
## Functions
### REM conversion
This function takes a pixelvalue (unprefixed), divides it with the $base value and returns a $rem value. Do not use 'px'.
#### The function code
@function REM($value)
  @return ($value / $base) + rem
#### SASS usage
width: REM(20)
#### Returns CSS
width: 1.25rem;

### RGBA helper
This function simplifies the rgba-writing. Hex value + opacity 0-1
#### The function code
@function RGBA($color, $opacity)
  @return (rgba($color, $opacity))
#### SASS usage
background: RGBA(#000, 0.9)
#### Returns CSS
background: rgba(0, 0, 0, 0.9);
