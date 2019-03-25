xaringanthemer CSS Help
================

Jump to: [Colors](#colors), [Sizes](#sizes), [Positioning](#positioning)

## About this vignette

This vignette cover some basic CSS, in particular to give an idea about
the possible values you can use with the various arguments to the
xaringan theme functions. See `vignette("template-variables", package =
"xaringanthemer")` for a full list of the theme functions.

Because we are setting the CSS properties from R, we can either give
**xaringanthemer** function arguments a character string or we can call
an R function or variable that returns a character string. For example,
we can create an R variable with a specific color that is used in
several places in a theme

``` r
firebrick <- "#CD2626"
style_xaringan(
  header_color = firebrick,
  link_color = firebrick
)
```

or we can directly give the character string

``` r
style_xaringan(
  header_color = "#CD2626",
  link_color = "#CD2626"
)
```

in both cases, we get CSS like the following that sets the link color

``` css
a, a > code {
  color: #CD2626;
}
```

Note that when a string is given to the theme function, the outer quotes
are removed.

In the sections below, R code is represented without quotes – like
`rgb(0.8, 0.15, 0.15)` – and CSS code is represented inside quotes –
like `"rgb(205, 38, 38)"` – to differentiate between R and CSS functions
with the same or similar names.

## Colors

In CSS, text colors are specified with the `color:` property, background
colors use `background-color:`, and border colors use `border-color:`.

In **xaringanthemer**, template variables that set

  - text color end with `_color`;
  - background color end with `_background_color`;
  - border color end with `_border_color`;

### Setting colors

In CSS, there are a number of ways to specify a color:

  - You can use a [color
    keyword](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#Color_keywords),
    such as `"darkslategray"` or `"red"`.

  - You can use the [RGB color
    specification](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#rgb\(\)_and_rgba\(\))
    either via
    
      - the hexadecimal representation
          - `"#CD2626"` or
          - `"#CD262680"` (50% transparency)
      - or the rgb function notation
          - `"rgb(255, 38, 38)"` or
          - `"rgba(255, 38, 28, 0.5)"` (50% transparency).

  - You can use the [HSL color
    specification](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#hsl\(\)_and_hsla\(\))
    via the functions
    
      - `"hsl(270,60%,70%)"` or
      - `"hsl(270, 60%, 50%, .15)"` (15% transparency).

In R, there are a number of ways to specify a color:

  - Use `rgb(205, 38, 38, maxColorValue = 255)` as an equivalent to the
    CSS `"rgb()"` function.
    
      - Without the `maxColorValue` argument, the `rgb()` function
        expects decimal numbers in the range \[0, 1\], like
        `rgb(0.8039, 0.1490, 0.1490)`.
      - The `rgb()` function also sets transparency via the `alpha`
        argument (in the \[0, 1\] range).

  - You can get the hexadecimal representation of a built-in R color
    using the `col2rgb()` function together with the `rgb()` function:
    
    ``` r
    rgb(t(col2rgb("red")), maxColorValue = 255)
    #> [1] "#FF0000"
    ```

## Sizes

In **xaringanthemer**, any template variable that accepts a CSS size (or
length unit) ends with `_size`. Sizes are also used for positioning and
those template variables end include `position` in their name.

There are many units available in CSS sizes, but the three most common
and easiest to use are pixels (`"px"`), percentage (`"%"`), and em units
(`"em"`). Mozilla’s devloper portal has a [full list of CSS length
units](https://developer.mozilla.org/en-US/docs/Web/CSS/length).

These sizes are either *absolute* or *relative* values. Relative values
are set relative to the size of the parent element, but absolute values
ignore the parent element.

  - Pixels `"px"`  
    Pixels are an *absolute* size unit, traditionally representing one
    device pixel. E.g. `"16px"` or `"23px"`.
  - Percentage `"%"`  
    Percentages are relative to the size of the parent element, scaled
    linearly. E.g. `"75%"` or `"150%"`.
  - em Units `"em"`  
    em Units are just like percentages, except expressed as decimals.
    E.g. `"0.75em"` or `"1.5em"`.

To make this more concrete, here is a simple “page” containing a section
header and two paragraphs.

``` html
<div class="page">
  <h1>Section 1</h1>
  <p>This is paragraph 1...</p>
  <p>This is paragraph 2...</p>
</div>
```

Intuitively, you might want the section header to have a somewhat bigger
font size than the paragraph text, but you don’t want to have to set the
text size for each and every paragraph or header.

To do this, we can set the base size of any element inside the `<div
class="page">`, and adjust the header size relatively.

``` css
.page {
  font-size: 16px;
}
h1 {
  font-size: 2em;
}
```

Now our paragraph font will be 16 pixels tall, and the level 1 headers
will be twice as big. If we later decide to change the base font size,
say to `"15px"`, the header text will still be twice as big as the
paragraph text.

## Positioning

If you’re reading this, you’re probably wondering how you make an
element be *where you want it to be*.

There are 3 items that **xaringanthemer** can help you position:

  - `background_position` (background image position)
  - `title_slide_background_position` (title slide background image
    position)
  - `footnote_position_bottom` (footnote location from bottom of screen)

### Footnote Position

**xaringanthemer** provides one template variable to adjust the position
of the footnote element. Footnotes can be insterted into a slide using
the `.footnote[Here's my quick footnote]` syntax.

The `footnote_position_bottom` argument adjust how far from the bottom
of the slide the footnote appears. The default value is `"3em"`, but you
can adjust this value up or down to get the footnote where you want.

### Background Position

The background position is set using the theme function arguments that
end with `background_position`. See [this article on
background-position](https://developer.mozilla.org/en-US/docs/Web/CSS/background-position#Syntax)
from Mozilla for more information.

Try any of the following values to get started:

``` r
background_position = "center"
background_position = "top"
background_position = "left"
background_position = "bottom"
background_position = "25% 75%" # X-value (from left) Y-value (from top)
background_position = "bottom 10px right 20px" # 10px from bottom, 20px from right
background_position = "top left 10px" # at top but adjusted left 10px
```

### General Positioning

Read this section if you want to put a slide element at a specific spot
on your slide.

The `position` CSS element is used to specify where an element is
located on the screen. Mozilla provides a [very good reference on
positioning](https://developer.mozilla.org/en-US/docs/Web/CSS/position)
that I’ve summarized here.

An element can be `"relative"`-ly positioned, `"absolute"`-ly
positioned, `"fixed"`, `"sticky"` or `"static"` (default). For an
element with a computed position (i.e. not `"static"`), you can also
specify the `top`, `right`, `bottom`, and `left` CSS properties for that
element. The `top`/`bottom` parameters specify vertical displacement,
and the `right`/`left` specify horizontal displacement.

  - Relatively positioned `position: relative`  
    For relatively positioned elements, the element position is adjusted
    relative to where it *would have been* if it were `static`.
  - Absolutely positioned `position: absolute` or `position: fixed`  
    Absolutely positioned elements are positioned relative to the block
    that contains the element (called a containing block). A `fixed`
    element won’t move with scrolling (but `fixed` is not recommended
    for remarkjs slides).

If you want something to appear in a specific position on your slide,
you’ll need to use the `extra_css` argument of the **xaringanthemer**
functions. For example, lets say you want a 300px by 300px box to appear
on the right side of your slide, you’ll need to create a special css
class:

``` r
style_xaringan(
  extra_css = list(
    ".box-right" = list(
      "height" = "300px",
      "width" = "300px",
      "position" = "absolute",
      "top" = "33%",
      "left" = "65%"
    )
  )
)
```

This creates CSS like this:

``` css
.box-right {
  height: 300px;
  width: 300px;
  position: absolute;
  top: 33%;
  left: 65%;
}
```

which you can then use in your slides by wrapping the slide content in
`.box-right[]`.

    .box-right[
    Stuff inside the box
    ]
