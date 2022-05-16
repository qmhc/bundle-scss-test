# Bundle Scss Test

## The Problem

When respectively import `src/style/button.scss` and `src/style/select.scss`, the `src/style/vars.scss` styles will be bundled twice in the output `dist/style.css`.

```scss
// src/style/vars.scss
$color-primary: #339af0;

:root {
  --color-primary: #{$color-primary};
}
```

```css
/* dist/style.css */
:root{--color-primary: #339af0}
/* ... */
:root{--color-primary: #339af0}
```

## Test Case 1

Directly import `src/style/index.scss`, the `src/style/vars.scss` only be bundled once.

```scss
// src/style/index.scss
@forward './button.scss';
@forward './select.scss';
```

## Test Case 2

Toggle the style `&--m1 &__el` in `src/style/button.scss`, the `src/style/vars.scss` only be bundled once.

```scss
// src/style/button.scss
@use './vars.scss';

.button {
  color: var(--color-primary);

  // comment out this style
  // &--m1 &__el {
  //   opacity: 60%;
  // }

  &--m2 &__el {
    opacity: 60%;
  }
}
```

Change the style `&--m1 &__el` defference to `&--m2 &__el`, the `src/style/vars.scss` only be bundled once.

```scss
// src/style/button.scss
@use './vars.scss';

.button {
  color: var(--color-primary);

  // change this style value
  &--m1 &__el {
    opacity: 0.6; // also the truth value of 0.6 is same to 60%, there will wrok normally
  }

  &--m2 &__el {
    opacity: 60%;
  }
}
```
