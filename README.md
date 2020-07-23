# simple-scss-grid

A light weight responsive grid system written in scss.

## Media Queries

The grid mixins use mini media query library, which are available for general use as well.

### Media Query Variables

Edit the \_media-query.scss file to configure the breakpoints as required:

```scss
$breakpoints: (
  'xs': 0,
  'sm': 576,
  'md': 768,
  'lg': 1024,
  'xl': 1440,
);
```

A print breakpoint allows rules to automatically be added for that breakpoint (and below):

```scss
$printBreakpoint: map-get($breakpoints, 'md');
```

### Media Query Mixins

#### mq

Creates a basic media query, by passing one of the registered breakpoint keys such as 'sm', 'md' etc

```scss
.foo {
  @include mq('sm') {
    //...
  }
}
```

css output:

```css
@media print, screen and (min-width: 576px) {
  .foo {
  }
}
```

#### mqLessThan

Use mqLessThan(bp) to target sizes below the passed in breakpoint

```scss
.bar {
  @include mqLessThan('md') {
    //...
  }
}
```

css output:

```css
@media print, screen and (max-width: 767px) {
  .bar {
  }
}
```

#### mqBetween

Target sizes between two breakpoints

```scss
.baz {
  @include mqBetween('md', 'lg') {
    //...
  }
}
```

css output:

```css
@media screen and (min-width: 768px) and (max-width: 1023px) {
  .baz {
  }
}
```

## Grid

The Grid uses 3 basic mixins: gridContainer, gridRow, and gridColumn

### Grid Variables

Configure variables in \_grid.scss to adjust default grid behaviour

The number of columns:

```scss
$colCount: 12;
```

Max width for container:

```scss
$maxWidth: (1440 - 64 * 2) px;
```

Padding for the outer container

```scss
$gridPadding: (
  'xs': 16px,
  'md': 24px,
  'lg': 32px,
);
```

Gutter sizes between columns:

```scss
$gridGutters: (
  'xs': 16px,
  'md': 24px,
  'lg': 32px,
);
```

### Grid Mixins

#### gridContainer

Creates the grids outer container with outer padding, max-width etc.

```scss
.container {
  @include gridContainer;
}
```

Optionally pass in different max width:

```scss
@include gridContainer(324px);
```

Optionally pass in different max width and different padding. This is provided in case you need multiple grids with in a project.

```scss
@include gridContainer(
  324px,
  (
    'xs': 8px,
    'md': 12px,
    'lg': 24px,
  )
);
```

#### gridRow

Grid an inner container for grid items that offsets their margins at the various gutter sizes.

```scss
.row {
  @include gridRow();
}
```

css output:

```css
.row {
  display: flex;
  flex-wrap: wrap;
  margin: -8px;
}
@media print, screen and (min-width: 768px) {
  .row {
    margin: -12px;
  }
}
@media screen and (min-width: 1024px) {
  .row {
    margin: -16px;
  }
}
```

#### gridColumn(n)

Creates a grid column of width n columns

```scss
.column {
  @include gridColumn(6);
}
```

css output:

```css
.column {
  margin: 8px;
  flex-grow: 0;
  flex-shrink: 0;
  flex-basis: calc(50% - 16px);
  max-width: 50%;
}
@media print, screen and (min-width: 768px) {
  .column {
    margin: 12px;
    flex-grow: 0;
    flex-shrink: 0;
    flex-basis: calc(50% - 24px);
    max-width: 50%;
  }
}
@media screen and (min-width: 1024px) {
  .column {
    margin: 16px;
    flex-grow: 0;
    flex-shrink: 0;
    flex-basis: calc(50% - 32px);
    max-width: 50%;
  }
}
```

#### gridColumn(n)

Creates a grid column of width n columns

```scss
.column {
  @include gridColumn(6);
}
```

css output:

```css
.column {
  margin: 8px;
  flex-grow: 0;
  flex-shrink: 0;
  flex-basis: calc(50% - 16px);
  max-width: 50%;
}
@media print, screen and (min-width: 768px) {
  .column {
    margin: 12px;
    flex-grow: 0;
    flex-shrink: 0;
    flex-basis: calc(50% - 24px);
    max-width: 50%;
  }
}
@media screen and (min-width: 1024px) {
  .column {
    margin: 16px;
    flex-grow: 0;
    flex-shrink: 0;
    flex-basis: calc(50% - 32px);
    max-width: 50%;
  }
}
```

#### gridClass(\$baseClass)

Generates global grid styles for convenience. Base class defaults to 'grid'

```scss
@include gridClasses();
```

Generates the following classes:

in the form .(base class) .xs-n

which provide a column of width n at the respective breakpoint

```css
.grid {
  .xs-1 {
    // 1 column width @ 'xs'
  }

  //...
  .xs-12 {
    // 12 column width @ 'xs'
  }

  //...
  .lg-12 {
    // 12 column width @ 'lg'
  }
}
```

Classes to offset (move right) by n columns:

```css
.xs-offset-n
```

Classes to push (move left) by n columns:

```css
.xs-push-n
```

Note when pushing and pulling - you must 'fill' the width of the available columns

e.g. for 12 column layout

.xs-8 and .xs-push-2 wont work without .xs-offset-2 to 'fill' the available columns.
