# Link styling style guide

This guide relies on human-focused class names, the idea is to use class names that a human can understand at first sight and avoid confusion on what does or how to use a class name.

<!-- TOC -->

- [Link styling style guide](#link-styling-style-guide)
  - [Components](#components)
    - [How to use it](#how-to-use-it)
    - [Variations](#variations)
    - [States](#states)
      - [Disabled](#disabled)
      - [Dirty](#dirty)
      - [Multiple states with same styling](#multiple-states-with-same-styling)
  - [Utilities](#utilities)
  - [Contribute](#contribute)

<!-- /TOC -->

## Components

CSS is responsible for each component styling, so the idea is specify the component name, part and variation in the smallest amount of classes, this will help to reduce the amount of compiled CSS and HTML.

Syntax: `.[<Component-name>]-[<component-part>]`

Note that this syntax is using kebab naming convention.

### How to use it

Let's say we need to create a search box:

```css
.Search-box { /* ... */ }

.Search-box-input { /* ... */ }

.Search-box-icon { /* ... */ }
```

Note that `Search-box` is the component name, It is has meaning for humans (machines don't care), component names can have multiple words so the idea is to use kebab case always, component names starts with uppercase to differentiate it from utilities.

Our HTML will look like this:

```html
<label class="Search-box">
  <input class="Search-box-input" type="text" />
  <img class="Search-box-icon" />
</label>
```

### Variations

Variations are extra classes to change the visual styling of the component.

Syntax: `.is-[<variation>]`

i.e: Let's use a button as an example, buttons are one of the components with the most variations in a website, normally we need to change size and button type (primary, secondary, with-icon):

```css
.Button { /* ... */ }

.Button.is-primary { /* ... */ }

.Button.is-big { /* ... */ }
```

And our HTML will look like this:

```html
<button class="Button is-primary is-big" ></button>
```

This is highly composable and verbose, is similar to BEM but without the word Button on every class, this will generate less CSS and HTML, also will maintain the HTML cleaner, so win/win.

Please don't create global variations to avoid issues with other components, if you need something like global variations, use [utilities](#utilities).

### States

A component state is needed in CSS, HTML and Javascript, so there is no need to handle this using CSS classes, HTML do this using attributes so we are going to use the same approach using `data-` attributes for non-native states.

Using the search box again, let's say we need to add some states (disabled, dirty).

#### Disabled

Disabled is a native state so we are going to use the `disabled` attribute:

```HTML
<label class="Search-box">
  <input class="Search-box-input" type="text" disabled/>
  <img class="Search-box-icon" />
</label>
```

Same in the CSS:

```css
.Search-box-input:disabled { /* ... */ }
```

#### Dirty

Dirty is not a native state, here we can use `data-dirty`, there is no need to pass a value for boolean attributes:

```HTML
<label class="Search-box">
  <input class="Search-box-input" type="text" data-dirty/>
  <img class="Search-box-icon" />
</label>
```

So in CSS we have the attribute selector, why not use it?

```css
.Search-box-input[data-dirty] { /* ... */ }
```

#### Multiple states with same styling

Sometimes we need the same styling in multiple states, normally we achieve this adding or removing classes but most of the time we have conflicts because the CSS specificity, using HTML attributes we can share properties between states easily using only CSS (and this is really useful in complex animations, specially debugging), but here are some points to follow when writing CSS:

- Put shared styling first
- Avoid nested selectors
- If you are writing a lot of weird CSS, something else is wrong, don't try to patch markup errors using CSS.

Back to our search box component, let's say that our `dirty` and `valid` states share some properties but `valid` will have extra properties, our CSS will look something like this:

```css
.Search-box-input[data-dirty],
.Search-box-input[data-valid] { 
  /* Shared styling */
}

.Search-box-input[data-dirty] {
  /* Specific to state styling */
}
```

If we put the specific to state styling first, we will have problems trying to overwrite the shared styling`

## Utilities

Utilities are global classes to handle global cases, like aria-hidden or hidden elements.

Syntax: `.[<utility-name>]`

Looks similar to Components but without the uppercase letter, this is the way to differentiate from components

i.e:

```css
.aria-hidden { /* ... */ }
.hidden { /* ... */ }
```

## Contribute

This is a personal project and a live guide, if you have suggestions on how to improve it, please [open a issue](https://github.com/linkstrifer/Hyrule-code-styleguide/issues/new) to discuss it or [send a pull request](https://github.com/linkstrifer/Hyrule-code-styleguide/compare).
