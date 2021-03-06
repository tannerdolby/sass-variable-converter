# Sass Variable Converter
A utility for converting Sass [variables](https://sass-lang.com/documentation/variables) to CSS [custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties).

## Usage
Convert a single Sass variable to a CSS custom property, or provide comma separated Sass variables as input to `vars.create()` to generate multiple custom properties at once.

First define variables so we can pass the module namespace to where the mixin will be receiving a Sass variable to convert. This way, we always have access to `meta.module-variables(module)` when using the `vars.create()` mixin and `vars.use()` function.

```scss
// vars/_variables.scss

$width: 50px;
$height: 25px;
$color: #f06; 
```

then load the stylesheet:

```scss
// vars/_index.scss

@forward "vars";
```

now simply include the mixins/variables with `@use "vars"` and utilize the `create` mixin and `use` function to convert Sass variables to custom properties:

```scss
// demo.scss

@use "vars";

$width: vars.$color;
$height: vars.$length;
$color: vars.$color; 

/* Convert a single Sass variable */
.demo-one {
    @include vars.create($width);
    @include vars.create($height);
    @include vars.create($color);

    width: vars.use($width);
    height: vars.use($height);
    color: vars.use($color);
}

/* Converting many Sass variables at once */
.demo-many {
    @include vars.create($width, $height, $color);

    width: vars.use($width);
    height: vars.use($height);
    color: vars.use($color);
}
```

compiling too:

```css
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

If a custom property is undefined, the default [fallback](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties#custom_property_fallback_values) value for each CSS custom property will be the Sass variables, unless an optional second argument is passed to `vars.use()` like this `vars.use($var, $customFallback)`.

```scss
.demo-many { 
    @include vars.create($width, $height, $color);

    width: vars.use($width, 40px);
    height: vars.use($height, 20px);
    color: vars.use($color, #111);
}
```
compiling too:

```css
.demo-many {
    --width: 50px;
    --height: 25px;
    --color: #f06;
    width: var(--width, 40px);
    height: var(--height, 20px);
    color: var(--color, #111);
}
```

## Usage options
Two utilities are available:

- `vars.create()` - A sass mixin for converting a Sass variable to a CSS custom property. Accepts a single Sass variable as input or multiple comma separated Sass variables.
- `vars.use()` - A sass function for returning the custom property. Accepts two arguments: a single Sass variable, and an optional second argument for a custom fallback value.


## Contributing 
I want this to be a helpful tool for the Sass community and anyone needing to convert Sass variables to CSS custom properties, so feel free to suggest any problems, features, or improvements in the [issue tracker](https://github.com/tannerdolby/sass-variable-converter/issues). 

See [CONTRIBUTING.md](https://github.com/tannerdolby/sass-variable-converter/blob/master/CONTRIBUTING.md) for more on contributing to this project.

## Maintainer
[@tannerdolby](https://github.com/tannerdolby)