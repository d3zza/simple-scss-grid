# simple-scss-grid

A light weight responsive grid system written in scss. 

## Media Queries

Grid uses sass-mq as a lightweight sass media query library. @see https://github.com/sass-mq/sass-mq

### Breakpoint settings

See the \_media-query.scss file to configure the breakpoint settings:

```scss
$breakpoints: (
  xs: 0px,
  sm: 576px, // 36em
  md: 768px, // 48em
  lg: 992px, // 62em
  xl: 1200px // 75em
);
```


## Grid

The Grid uses 3 basic mixins: gridContainer, gridRow, and gridColumn

### Grid Variables

Configure variables in $defaultGridConfig  \_grid.scss to adjust default grid behaviour

```scss
$defaultGridConfig: (
  'gridGutters': (
    xs: (
      x: 20px,
      y: 20px,
    ),
    md: (
      x: 24px,
      y: 24px,
    ),
    lg: (
      x: 32px,
      y: 32px,
    ),
  ),
  'gridPadding': (
    xs: 16px,
    md: 24px,
    lg: 32px,
  ),
  'colCount': 12,
  'maxWidth': (
    1440 - 64 * 2,
  ),
);
```


Note: All of the grid mixins accept an optional grid config which is merged with the default settings so you can provide overrides to defaults on a case by case basis

The number of columns:

```scss
colCount: 12;
```

Max width for container:

```scss
maxWidth: (1440 - 64 * 2) px;
```

Padding for the outer container

```scss
gridPadding: (
  xs: 16px,
  md: 24px,
  lg: 32px,
);
```

Gutter sizes between columns:

```scss
  'gridGutters': (
    xs: (
      x: 20px,
      y: 20px,
    ),
    md: (
      x: 24px,
      y: 24px,
    ),
    lg: (
      x: 32px,
      y: 32px,
    ),
  )
```

### Grid Mixins

#### gridContainer

Creates the grids outer container with outer padding and max-width settings.

```scss
.container {
  @include gridContainer;
}
```

Optionally pass in different config (to override default max width and padding).

```scss
@include gridContainer( $gridConfig );
```

#### gridRow

Grid an inner flex container for grid items that offsets their margins at the various gutter sizes.

```scss
.row {
  @include gridRow;
}
```

Optionally pass in different config (to override gutters)

```scss
@include gridRow( $gridConfig );
```


css output:

```css
.row {
  display: flex;
  flex-wrap: wrap;
  margin-top: -8px;
  margin-right: -8px;
  margin-bottom: -8px;
  margin-left: -8px;
}
@media print, screen and (min-width: 768px) {
  .row {
    margin-top:-12px;
    margin-right:-12px;
    margin-bottom:-12px;
    margin-left:-12px;
  }
}
@media screen and (min-width: 1024px) {
  .row {
    margin-top:-16px;
    margin-right:-16px;
    margin-bottom:-16px;
    margin-left:-16px;
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
  margin-top: 8px;
  margin-right: 8px;
  margin-bottom: 8px;
  margin-left: 8px;
  flex-grow: 0;
  flex-shrink: 0;
  flex-basis: calc(50% - 16px);
  max-width: 50%;
}
@media print, screen and (min-width: 768px) {
  .column {
    margin-top:12px;
    margin-right:12px;
    margin-bottom:12px;
    margin-left:12px;
    flex-grow: 0;
    flex-shrink: 0;
    flex-basis: calc(50% - 24px);
    max-width: 50%;
  }
}
@media screen and (min-width: 1024px) {
  .column {
    margin-top:16px;
    margin-right:16px;
    margin-bottom:16px;
    margin-left:16px;
    flex-grow: 0;
    flex-shrink: 0;
    flex-basis: calc(50% - 32px);
    max-width: 50%;
  }
}

Optionally pass in different config (to override gutters)

```scss
@include gridColumn( n, $gridConfig );
```

#### gridClasses($baseClass: 'grid', $gridConfig: ())

Generates global grid styles for convenience. Base class defaults to 'grid'. Optionally pass alt config.

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
