# FlexLayout: Arranging div as table on specific breakpoint

I've encountered a layout requirement to reorganize divs to table when in mobile view.

**Visual Layout**

Large Screen:
![alt text][layout-large-screen]

[layout-large-screen]: https://github.com/jengfad/hyperinfinitemonkey/raw/master/src/images/01.layout-large-screen.PNG ""

Small Screen:
![alt text][layout-small-screen]

[layout-small-screen]: https://github.com/jengfad/hyperinfinitemonkey/raw/master/src/images/01.layout-small-screen.PNG ""

Here are the specifics:

**Large Screen**: The yellow div holds green divs arranged in a row. Green divs holds 2 red divs on a column.

**Small Screen**: The yellow div holds green divs arranged in a column. Green divs holds 2 red divs on a row.

Notice that the two resolutions have opposite behavior when arranging the green and red divs. Since flex-layout is already integrated, this should be easier with the help of breakpoints and ngClass.

Here is the code:

**HTML**
```
<div fxLayout="row" fxLayout.xs="column" class="table" ngClass.xs="table-mobile">
  <div fxLayout="column" fxLayout.xs="row" class="row">
    <div class="cell">Title A:</div>
    <div fxFlex class="cell">Alpha Content</div>
  </div>
  <div fxLayout="column" fxLayout.xs="row" class="row">
    <div class="cell">Title B:</div>
    <div fxFlex class="cell">Bravo Content</div>
  </div>
  <div fxLayout="column" fxLayout.xs="row" class="row">
    <div class="cell">Title C:</div>
    <div fxFlex class="cell">Charlie Content</div>
  </div>
  <div fxLayout="column" fxLayout.xs="row" class="row">
    <div class="cell">Title D and unusually long string:</div>
    <div fxFlex class="cell">Delta Content the quick brown fox jumps over the lazy dog</div>
  </div>
</div>

```

**CSS**
```
/**main**/
.table-mobile {
	display: table !important;
}

.table-mobile .row {
	display: table-row !important;
}

.table-mobile .cell {
	display: table-cell;
}
```

Here are the important points to note:

A. HTML
- We have three div classes: 'table', 'row', 'cell'. This will serve as styling classes as well as segments of our markup.
- For the large screen, we told flex-layout the following:
        1. 'table' is arranged into row.
        2. 'row' is arranged into colum.
    This is done thru the `fxLayout="row"` and `fxLayout="column"` tags on the divs.

- For the small screen, we told flex-layout the vice-versa of large screen.
On this one, we will be using a small screen breakpoint: `fxLayout.xs="row"`. `.xs` denotes that this will be triggered when the screen resolution has `max-width: 599px`. You can see details on breakpoints in [here](https://github.com/angular/flex-layout/wiki/Responsive-API).

- Since the requirement wants to enforce a table-like layout on small screen, we used 'table-mobile' css class on .xs breakpoint: `ngClass.xs="table-mobile"`.

- `fxFlex` was used to equalize the height of the divs when there are uneven spaces left. This will be visible when on large screen.

B. CSS

- We have 3 rules for the 3 segments of our markup that mimics the behavior of `<table>` element.
- `!important` tag was used to override the flex-layout styles. Note that in using `fxLayout` directives, it will automatically attached various styling attributes.

Here is the [plunkr demo](https://plnkr.co/edit/7rsHjF1T58exGZch2gAb?p=preview).

As you play around, try the following:

- on large screen, remove `fxFlex`
- on small screen, remove `ngClass.xs="table-mobile`
