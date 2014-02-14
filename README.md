
# Principles of writing consistent, idiomatic Sass

- [1. Acknowledgements](#1-acknowledgements)
- [2. Definitions](#2-definitions)
    - [Object](#object)
    - [Children](#children)
    - [Module](#module)
    - [Package](#package)
    - [Block](#block)
    - [Element](#element)
    - [Modifier](#modifier)
- [3. General principles](#3-general-principles)
- [4. OOSass](#4-oosass)
- [5. Selector performance](#5-selector-performance)
- [6. BEM (Naming Convention)](#6-bem-naming-convention)
- [7. Whitespace](#7-whitespace)
- [8. Comments](#8-comments)
- [9. Properties](#9-properties)
    - [Declaration order](#declaration-order)
    - [Exceptions and slight deviations](#exceptions-and-slight-deviations)
- [10. File Structure](#10-file-structure)
- [11. Functions](#11-functions)
- [12. Mixins](#12-mixins)
- [13. Practical example](#13-practical-example)

## Introduction

The following document outlines an opinionated but reasonable style guide for SCSS development, strongly encouraging the use of existing, common, sensible patterns.

While many people think of Sass as 'just CSS' there are many unique challenges that need to be addressed if we want to be able to write the best possible code.

The role of this document is to share a coding style that will help you share code with others and avoid a common pitfalls.

> "Arguments over style are pointless. There should be a style guide, and you should follow it"

Rebecca Murphey

## 1. Acknowledgements

This guide is heavily based on:

* [Nicolas Gallagher's Idiomatic Css](https://github.com/necolas/idiomatic-css) ([CC BY 3.0 licensed](http://creativecommons.org/licenses/by/3.0/))
* [Anthony Short's Idiomatic Sass](https://github.com/anthonyshort/idiomatic-sass)

It is also influenced by:

* [Jonathan Snook's SMACSS](http://smacss.com/)
* [Nicole Sullivan's OOCSS](https://github.com/stubbornella/oocss)


## 2. Definitions

### Object

A single piece of the design, usually fairly small. This could be things like `.message`, `.block`, `.list`, `.post`. Objects should be independent. Think of them as lego blocks. Objects have "modifiers" and "children".

### Children

If an "object" is the parent, any sub-parts of that object are considered its children. Children are only ever controlled by the object it belongs to. A `.message` object may have a title that it styles, `.message__title`, this would be referred to as a child element.

### Module

A single piece of functionality that can be composed of CSS, mixins, functions and assets (such as images or fonts). A module has a single entry point and a single purpose and role. eg. A grid framework could be a module.

### Package

When a module is shared with others via a package manager like Bower it will generally be referred to as a package. This means that the term "module" and "packages" are fairly interchangable.

### Block

This is another term for the concept of an "object".

### Element

When referring to "objects" and "blocks", the word "element" is interchangable with the word "children".

### Modifier

"Objects" may be modified in a way that changes their style in small ways, think of them as themes or alternative styles. For example, a `.list` object may have a `.list--small` modifier to make the text smaller.

## 3. General principles

> "Part of being a good steward to a successful project is realizing that
> writing code for yourself is a Bad Idea™. If thousands of people are using
> your code, then write your code for maximum clarity, not your personal
> preference of how to get clever within the spec." - Idan Gazit

* Don't try to prematurely optimize your code; keep it readable and
understandable.
* All code in any code-base should look like a single person typed it, even
when many people are contributing to it.
* Strictly enforce the agreed-upon style.
* If in doubt when deciding upon a style use existing, common patterns.


## 4. OOSass

Writing good Sass code starts with correctly dividing and modularizing your objects. It is arguably more important than any other aspect of writing CSS.

* Break functionality into smaller objects
* Objects should never manipulate other objects. eg. `.message` would never change the style of a nested object called `.list`. Instead use child selectors like
`.message__list` and use both classes in the markup  `<div class="list message__list">` or use a modifier `<div class="message"><div class="list list--small"></div></div>`
* Never, ever use location-based styling. This means a block is never styled different because it is within another block. Objects should have "modifiers" instead of location-related styles `.block--large {}` instead of `#sidebar  block {}`
* Separate layout from style. This means an object that handles background and border won't control padding and margin. Styles generally fall into a couple of categories: layout, texture, typography. Each object should generally only handle of these. But be pragmatic about it and consider reusablity at all times.

## 5. Selector performance

* Use classes for the bulk of your selectors (see [BEM](#BEM) below)
* Use care when nesting unqualified element selectors inside a Block or Element
selector eg `.block__element a` as this can confer too much specificity. Consider adding a class to the element `.block__element-link` instead, or using an immediate child selector to reduce the depth of applicability `.block__element < a`
* **Never** use IDs for styling: they carry to much specificity. While this may
seem extreme it is easy to enforce and will save you pain in the long run.
* Similarly **never** use `name` values or aria role attributes for styling.
* Remember that CSS selectors are evaluated from right -> left;
* Educate yourself about selector performance: it's part of your job
* No really, **never** use IDs

Enforce these rules by using one of the naming conventions in the next section.

In general the principle is that:

- Classes are for styling.
- Name values are for the server.
- Aria roles are for accessibility
- Data attributes are for JavaScript (or icons).
- IDs are for losers.


## 6. BEM (Naming Convention)

Use the [BEM](http://bem.info) pattern consistently when naming your classes

```scss
.block-name {}
.block-name__child-name {}
.block-name--modifier {}
```

## 7. Whitespace

Only one style should exist across the entire source of your code-base. Always be consistent in your use of whitespace. Use whitespace to improve readability.

* _Never_ mix spaces and tabs for indentation.
* Use only soft indents (spaces). Stick to this without fail. (Ideally enforce
with a precomit hook)
* Use 4 space characters used per indentation level.

Tip: configure your editor to "show invisibles" or to automatically remove end-of-line whitespace.

Tip: use an [EditorConfig](http://editorconfig.org/) file (or equivalent) to help maintain the basic whitespace conventions that have been agreed for your code-base.


## 8. Comments

Well commented code is extremely important. Take time to describe components, how they work, their limitations, and the way they are constructed. Don't leave others in the team guessing as to the purpose of uncommon or non-obvious code.

Comment style should be simple and consistent within a single code base.

* Place comments on a new line above their subject.
* Keep line-length to a sensible maximum, e.g., 80 columns.
* Make liberal use of comments to break (S)CSS code into discrete sections.
* Use "sentence case" comments and consistent text indentation.

Tip: configure your editor to provide you with shortcuts to output agreed-upon
comment patterns.

Always use Sass comments (`// comment`) in preference to CSS comments (`/* comment */`) as they will not be outputed. Only use CSS comments if you have a good reason for the comment to remain in the generated output (eg
license restrictions).

Example:

```scss
// ============================================================================
// Section comment block
// ============================================================================

// Sub-section comment block
// ============================================================================

///
// Short description using Doxygen-style comment format
//
// The first sentence of the long description starts here and continues on this
// line for a while finally concluding here at the end of this paragraph.
//
// The long description is ideal for more detailed explanations and
// documentation. It can include example HTML, URLs, or any other information
// that is deemed necessary or useful.
//
// @tag This is a tag named 'tag'
//
// TODO: This is a todo statement that describes an atomic task to be completed
//   at a later date. It wraps after 80 characters and following lines are
//   indented by 2 spaces.
///

// Basic comment
```

## 9. Properties

Formatting of selectors is important and is the key to making code easy to share amongst a team. You should follow the rules from [Idiomatic CSS](https://github.com/necolas/idiomatic-css):

* Use one discrete selector per line in multi-selector rulesets.
* Include a single space before the opening brace of a ruleset.
* Include one declaration per line in a declaration block. This allows for better integration with version control.
* Use one level of indentation for each declaration.
* Include a single space after the colon of a declaration.
* Use lowercase and shorthand hex values, e.g., #aaa.
* Use single or double quotes consistently. Preference is for double quotes, e.g., content: "".
* Quote attribute values in selectors, e.g., input[type="checkbox"].
* Where allowed, avoid specifying units for zero-values, e.g., margin: 0.
* Include a space after each comma in comma-separated property or function values.
* Include a semi-colon at the end of the last declaration in a declaration block.
* Place the closing brace of a ruleset in the same column as the first character of the ruleset.
* Separate each ruleset by a blank line.
* Limit nesting to 2 level deep. Reassess any nesting more than 3 levels deep. This is a sign of bad CSS as selectors become too specific.
* Avoid large numbers of nested rules. Break them up when readability starts to be affected. Preference to avoid nesting that spreads over more than 25 lines.


```scss
.selector-1,
.selector-2,
.selector-3[type="text"] {
-webkit-box-sizing: border-box;
-moz-box-sizing: border-box;
box-sizing: border-box;
display: block;

font-family: helvetica, arial, sans-serif;

color: #333;
background: #fff;
background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
}

.selector-a,
.selector-b {
padding: 10px;
}
```

### Declaration order

The basic rule of thumb is at-rules, properties, then blocks.

1. `$variable` should **always** appear at the top.
2. `@extend` should always appear before properties. It's like extending a class in a conventional OO language.
3. `@include` should appear second, except in the case of `breakpoint()`
or `respond-to()` (see below). This allows the properties to override the mixins.
4. Properties should appear after this, optionally grouped by type.
5. Mixins with content blocks should appear next. `@include someMixin { properties }`
6. Selectors that target itself. `&.modifier`
7. Child selectors appear last.
8. Blocks within selectors should be separated by a single line follow the same rules.
9. `breakpoint()`/`respond-to()` should be listed in mobile-first order (ie smallest to largest) at the end of the declaration set but before any nested rules.

Cluster related properties (e.g. positioning and box-model) together.

```scss
.selector {
$bg: blue;
$fallback: green;

@extend .clearfix;
@include border-box;

height: 200px;
width: 200px;

text-decoration: none;

@include after {
    position: absolute;
}

&.selector--modifier {
    background: red;
}

@include respond-to('medium and above') {
    background: green;
}
@include respond-to('large and above') {
    background: yellow;
}
.selector__child {
    display: none;
}
}
```


### Exceptions and slight deviations

Large blocks of single declarations can use a slightly different, single-line
format. In this case, a space should be included after the opening brace and
before the closing brace.

```scss
.selector-1 { width: 10%; }
.selector-2 { width: 20%; }
.selector-3 { width: 30%; }
```

Long, comma-separated property values - such as collections of gradients or
shadows - can be arranged across multiple lines in an effort to improve
readability and produce more useful diffs. There are various formats that could
be used; one example is shown below.

```scss
.selector {
    background-image:
            linear-gradient(#fff, #ccc),
            linear-gradient(#f3c, #4ec);
    box-shadow:
            1px 1px 1px #000,
            2px 2px 1px 1px #ccc inset;
}
```


## 10. File Structure

* Each logical module of code should belong in its own file. Avoiding putting multiple objects in the same file. This allows you to use the filesystem to navigate your Sass rather than relying on comment blocks.
* Mixins/placeholders/functions should, if possible, belong in their own file, within a module directory if they pertain to a module.
* Files should be named for the component they are housing. A `block-list` object will live in a `_block-list.scss` file.

## 11. Functions

* Functions should **always** be prefixed with a namespace, which should match the module name: `grid-columns()`
* Private functions should be prefixed with an underscore, followed by the namespace: `_grid-columns()`
* Functions should be documented using Sass-flavoured DocBlock

## 12. Mixins


* Mixins should be prefixed with their module namespace: `@mixin grid-builder()`
* Mixins should not be not longer than ~50 lines
* Private mixins that are not used outside of the current file should be prefixed with an underscore: `@mixin _grid-helper`
* Avoid using more than 4 parameters. It is a sign that a mixin is too complex.Pass a list instead
* Mixins should be documented using Sass-flavoured DocBlock

```scss
// Loop through each breakpoint and build
// classes for each using the breakpoint mixins
// First breakpoint is no media query — mobile-first.
//
// @param {List} $breakpoints List of column breakpoints
// @param {Boolean} $spacing Include spacing classes?
// @param {Boolean} $visibility Include visibilty classes?
// @param {Boolean} $layout Include layout classes?
// @api private
@mixin breakpoints($breakpoints, $spacing: true, $visibility: true, $layout: true) {
@each $columns in $breakpoints {
    @if index($breakpoints, $columns) == 1 {
        @include _breakpoint-classes($columns, $spacing, $visibility, $layout);
    }
    @else {
        @include grid-from($columns) {
            @include _breakpoint-classes($columns, $spacing, $visibility, $layout);
        }
    }
}
}
```

## 13. Practical example

An example of various conventions.

```scss
// ============================================================================
//   Grid layout
//   ==========================================================================

/**
* Column layout with horizontal scroll.
*
* This creates a single row of full-height, non-wrapping columns that can
* be browsed horizontally within their parent.
*
* Example HTML:
*
* <div class="grid">
*     <div class="cell cell-3"></div>
*     <div class="cell cell-3"></div>
*     <div class="cell cell-3"></div>
* </div>
*/
```



