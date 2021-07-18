# Sass Variable Converter
A utility for converting Sass [variables](https://sass-lang.com/documentation/variables) to CSS [custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties).

## Usage
This utility is still in the beginning stages, it can accomplish converting a single Sass variable to a CSS custom property, or a multiple comma seperated Sass variables.

First define variables so we can pass the module namespace to where the mixin will be receiving a Sass variable to convert. This way, we always have access to `meta.module-variables(module)` when using the `var.create()` mixin and `var.use()` function.

```scss
// var/_variables.scss

$width: 50px;
$height: 25px;
$color: #f06; 
```

then load the stylesheet:

```scss
// var/_index.scss

@forward "vars";
```

now simply include the mixins/variables with `@use "var"` and utilize the `create` mixin and `use` function to convert Sass variables to custom properties:

```scss
// demo.scss

@use "var";

$width: var.$color;
$height: var.$length;
$color: var.$color; 

// Converting a single Sass variable at a time
.demo-one {
    @include var.create($width);
    @include var.create($height);
    @include var.create($color);

    width: var.use($width);
    height: var.use($height);
    color: var.use($color);
}

// Converting many Sass variables at a time
.demo-many {
    @include var.create($width, $height, $color);

    width: var.use($width);
    height: var.use($height);
    color: var.use($color);
}
```

compiles to:

```scss
.demo {
    --width: 50px;
    --height: 25px;
    --color: #f06;
    width: var(--width, 50px);
    height: var(--height, 25px);
    color: var(--color, #f06);
}

.demo-many {
    --width: 50px;
    --height: 25px;
    --color: #f06;
    width: var(--width, 50px);
    height: var(--height, 25px);
    color: var(--color, #f06);
}
```

## Custom fallback values

If a custom property is undefined, the default [fallback](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties#custom_property_fallback_values) value for each CSS custom property will be the Sass variables, unless an optional second argument is passed to `var.use()` like this `var.use($var, $customFallback)`.

```scss
.demo-many { 
    @include var.create($width, $height, $color);

    width: var.use($width, 40px);
    height: var.use($height, 20px);
    color: var.use($color, #111);
}
```
compiles to:

```scss
.demo-many {
    --width: 50px;
    --height: 25px;
    --color: #f06;
    width: var(--width, 40px);
    height: var(--height, 20px);
    color: var(--color, #111);
}
```

## Contributing
I want this to be a helpful tool for anyone needing to convert Sass variables to CSS custom properties, so feel free to suggest any features, improvements, changes in the [issue tracker](https://github.com/tannerdolby/sass-variable-converter/issues)

See [CONTRIBUTING.md](https://github.com/tannerdolby/sass-variable-converter/blob/master/CONTRIBUTING.md) for more on contributing to this project.

## Maintainer
[@tannerdolby](https://github.com/tannerdolby)