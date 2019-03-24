# DevMtn Phoenix Codebase Style Guide

## Introduction

If you come back to code in a few weeks or months, will you be able to work out what’s happening without needing to look at every line?

Sources: [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript), [Airbnb React/JSX Style Guide](https://github.com/airbnb/javascript/tree/master/react), [Get BEM](http://getbem.com/naming/), [Sass Style Guide](https://css-tricks.com/sass-style-guide/)


## Table of Contents

1. [Basic Rules](#basic-rules)
2. [Naming Conventions](#naming-conventions)
3. [Commenting](#commenting)
4. [Destructuring](#destructuring)
5. [BEM](#bem)
6. [CSS](#css)
7. [SASS](#sass)

## Basic Rules

---

- Only include one React component per file
- Always use JSX syntax
- Do not use React.createElement unless you’re initializing the app from a file that is not JSX

## Naming Conventions

---

- Avoid single letter names
- **Filenames**: Use PascalCase for filenames. E.g., UserDashboard.js

- **Components**: Use the filename as the component name. For example, Footer.js should have a reference name of Footer:

```javascript
import Footer from "./Footer/Footer";
```

- **Props**: Avoid using DOM component prop names for different purposes:

```javascript
  // bad
  <MyComponent style={'red'} />

  // bad
  <MyComponent className={'red'} />

  // good
  <MyComponent dangerColor={'red'} />
```

- **Variables**:

  - Declare all local variables with either `const` or `let`. Use `const` by default, unless a variable needs to be reassigned. **The var keyword must not be used**
  - Name variables based on what data it contains:

```javascript
// bad - expecting a string
const fullName = 42;

// bad - expecting a number
const age = "John Doe";

// good - expecting an array of numbers
const numArray = [25, 32, 64];
```

- **Functions**: Name functions in accordance with what they do:
```javascript
// bad
function addNumbers(a, b) {
  return a * b;
}
// good
function addNumbers(a, b) {
  return a + b;
}
```

## Commenting

---
- **Documentation comments**:
  - Explain what a block of code does for future self/developers
- **Clarification comments**:
  - Notes for future self/developers on why a code block works if it seems janky.  What does it cover, what purpose does it serve, why is respective solution used or preferred?
```javascript

Use // for single line comments

Use /** */
/** 
 * for
 * multi-line
 * comments
 * */

// Start all comments with a space to make it easier to read

/**
 * Use // FIXME: to annotate problems
 * Use // TODO: to annotate solutions to problems
 * */

```

## Destructuring

---

**Objects**: Use object destructuring when accessing and using a property multiple times

```javascript
 // bad - this.props.title is used more than once
 <h1>{this.props.title}</h1>
<img
  src={this.props.imageUrl}
  alt={this.props.title}
  />
// good
const { title } = this.props;
<h1>{title}</h1>
<img
  src={this.props.imageUrl}
  alt={title}
  />
```

**Arrays**:

```javascript
const numArr = [1, 2, 3, 4];

// bad
const first = numArr[0];
const second = numArr[1];

// good
const [first, second] = numArr;
```

## BEM
---

What is BEM? BEM stands for Block, Element, Modifier and is used to provide consistent structure to your css codebase. This structure makes sure all developers working on a website are writing similar, readable code.

#### Basic Rules:

- Declare blocks using a single or hyphenated name
```javascript
<div class="main" />
<div class="main-container" />
```
- Elements inside blocks are denoted by double underscore
```javascript
<div class="main">
  <p class="main__text" />
</div>
```
- Modifiers are denoted by double dashes
```javascript
<div class="main">
  <p class="main__text main__text--highlight" />
</div>
```

* Note how the base class, `main__text`, is kept in the modifer example

Correct BEM syntax simplifies your css and Sass. There are a few rules regarding each part of BEM:

#### **Blocks & Elements**:

- Use class name selector only
- No tag name or ids
- No dependency on other blocks/elements on a page

```javascript
//Good
.main__text {
  color: #042;
}

//Bad
.main .main__text {
  color: #042;
}
div.main__text {
  color: #042;
}
```

#### **Modifiers**:
- Modifiers are used when you need to change a specific element's style

```javascript
.block {
  color: blue;
  font-size: 24px;
 }
.block--modifer {
  color: orange;
}

```


- Use modifier class name as selector:

```css
.block--hidden {
  display: none;
}
```

- To alter elements based on a block-level modifier:

```css
.block--modifier .block__elem {
}
```

- Element modifier:

```css
.block__elem--modifier {
}
```

An example taken from [GetBEM's](http://getbem.com/naming/) website shows sample BEM'd html and what the css for it would look like:

#### HTML:

```html
<form class="form form--theme-xmas form--simple">
  <input class="form__input" type="text" />
  <input class="form__submit form__submit--disabled" type="submit" />
</form>
```

#### CSS:

```css
.form {
}
.form--theme-xmas {
}
.form--simple {
}
.form__input {
}
.form__submit {
}
.form__submit--disabled {
}
```

Sass actually makes this process even simpler! In the above example, they repeatedly wrote `.form`. We can eliminate this by using the [Sass Ampersand](https://css-tricks.com/the-sass-ampersand/).

```css
.form {
  &--theme-xmas { }
  &--simple { }
  &__input { }
  &__submit { }
  &__submit--disabled { }
}
```

The `&` in sass will compile into the parent selector, `.form`.

Now that we have a plan for naming our classes, lets talk about guidelines for css and Sass.

Source: [Get BEM](http://getbem.com/naming/)

## CSS
---
- Put declarations in alphabetical order in order to achieve consistent code in a way that is easy to remember and maintain.

```javascript
.block {
  background: fuchsia;
  border: 1px solid;
  -moz-border-radius: 4px;
  -webkit-border-radius: 4px;
  border-radius: 4px;
  color: black;
  text-align: center;
  text-indent: 2em;
}
```
- When using multiple selectors in a rule declaration, give each selector its own line.
```javascript
.one,
.selector,
.per-line {
  // ...
}
```


## SASS
---
 - Maximum Nesting: Three Levels Deep
  ```javascript
  .block {
    &__element {
      &--modifier {
        // no more!
      }
    }
  }
  ```
  - List @extend(s) First
```javascript
.block {
    @extend %module; 
  ...
}
  ```
  - List @include(s) Next
  ```javascript
  .block {
    @extend %module; 
    @include transition(all 0.3s ease-out);
    ...
}
```
  - List "Regular" Styles Next
  ```javascript
  .block {
    @extend %module;
    @include transition(all 0.3s ease-out);
    background: LightCyan;
    ...
}
```
  - Nested Pseudo Classes and Pseudo Elements Next
  ```javascript
  .block {
    @extend %module;
    @include transition(all 0.3s ease-out);
    background: LightCyan;
    &:hover {
      background: DarkCyan;
    }
    &::before {
      content: "";
      display: block;
    }
    ...
}
```
  - Nested Selectors Last
  ```javascript
  .block {
    @extend %module; 
    @include transition(all 0.3s ease);
    background: LightCyan;
    &:hover {
      background: DarkCyan;
    }
    &::before {
      content: "";
      display: block;
    }
    > h3 {
      @include transform(rotate(90deg));
      border-bottom: 1px solid white;
    }
}
```

  Source: [Sass Style Guide](https://css-tricks.com/sass-style-guide/)