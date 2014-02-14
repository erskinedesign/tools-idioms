# Principles of writing consistent, idiomatic HTML

- [Table of contents](#table-of-contents)
- [1. General principles](#1-general-principles)
- [2. Whitespace](#2-whitespace)
- [3. Format](#3-format)
    - [Exceptions and slight deviations](#exceptions-and-slight-deviations)
- [4. Attribute order](#4-attribute-order)
- [5. Naming](#5-naming)
- [6. Commenting](#5-commenting)
- [7. Practical example](#6-practical-example)
- [License](#license)

# Principles of writing consistent, idiomatic HTML

The following document outlines an opinionated reasonable style guide for HTML development. These guidelines strongly encourage the use of existing, common, sensible patterns.

## 1. General principles

* All code in any code-base should look like a single person typed it, no matter how many people contributed.
* Strictly enforce the agreed upon style.
* If in doubt when deciding upon a style, use existing, common patterns.

## 2. Whitespace

Only one style should exist across the entire source of your code-base. Always be consistent in your use of whitespace. Use whitespace to improve readability.

* _Never_ mix spaces and tabs for indentation.
* Use only soft indents (spaces). Stick to this without fail. (Ideally enforce with a precomit hook)
* Use 4 space characters used per indentation level.

Tip: configure your editor to "show invisibles" or to automatically remove end-of-line whitespace.

Tip: use an [EditorConfig](http://editorconfig.org/) file (or equivalent) to help maintain the basic whitespace conventions that have been agreed for your code-base.


## 3. Format

* Always use lowercase tag and attribute names.
* Write one discrete block level element per line.
* Two or more natural inline elements may be placed on the same line, but only where it does not reduce readability.
* Use one additional level of indentation for each nested element.
* Use valueless boolean attributes (e.g. `checked` rather than `checked="checked"`).
* Always use double quotes to quote attribute values.
* Omit the `type` attributes from `link` stylesheet, `style` and `script` elements.
* Always include closing tags.
* Always include a trailing slash in self-closing elements (eg `<hr/>` not `<hr>`).
* Keep line-length to a sensible maximum, e.g., 80 columns.

Example:

```html
<div class="tweet">
    <a href="path/to/somewhere">
        <img src="path/to/image.png" alt="">
    </a>
    <p>[tweet text]</p>
    <button disabled>Reply</button>
</div>
```

### Exceptions and slight deviations

Elements with multiple attributes can have attributes arranged across multiple lines in an effort to improve readability and produce more useful diffs.

Example:

```html
<a class="[value]"
    data-action="[value]"
    data-id="[value]"
    href="[url]">
        <span>[text]</span>
</a>
```

## 4. Attribute order

HTML attributes should be listed in an order that reflects the fact that class names are the primary interface through which CSS and JavaScript select elements.

1. `class`
2. `id`
3. `data-*`
4. Everything else

Example:

````html
<a class="[value]" id="[value]" data-name="[value]" href="[url]">[text]</a>
````

## 5. Naming

Naming is hard, but very important. It's a crucial part of the process of developing a maintainable code base, and ensuring that you have a relatively scalable interface between your HTML and CSS/JS.

* Use clear, thoughtful, and appropriate names for HTML classes. The names should be informative both within HTML and CSS files. See BEM syntax section in Idiomatic Sass
* Avoid _systematic_ use of abbreviated class names. Don't make things difficult to understand.

Example with bad names:

```html

<!-- antipattern, do not use-->
<div class="cb s-scr"></div>
```

```scss
// antipattern, do not use
.s-scr {
  overflow: auto;
}

.cb {
  background: #000;
}
```

Example with better names:

```html
<div class="grid__column is--scrollable"></div>
```

```scss
.is--scrollable {
    overflow: auto;
}

.grid__column {
    background: #000;
}
```

## 6. Commenting

* In general, HTML comments should be avoided as they lead to output bloat.
* Never use HTML comments to 'disable' a block of html from rendering, except perhaps when debugging.
* Where it adds clarity to your code, consider using trailing closing tag comments to indicate which element is being closed, as in this example:

```html
<div class="grid">
    <div class="grid__column grid__column--3">

        <!-- content-->

    </div><!--//.grid__column-->
    <div class="grid__column grid__column--9">
        <!-- some more content -->

    </div><!--//.grid__column-->
</div><!--//.grid-->
```

Consider stripping comments as part of your build step.


## 6. Practical example

An example of various conventions.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Document</title>
        <link rel="stylesheet" href="main.css">
        <script src="main.js"></script>
    </head>
    <body>
        <article class="post" id="1234">
            <time class="timestamp">March 15, 2012</time>
            <a  data-id="1234"
                 data-analytics-category="[value]"
                 data-analytics-action="[value]"
                 href="[url]">[text]</a>
            <ul>
                <li>
                    <a href="[url]">[text]</a>
                    <img src="[url]" alt="[text]">
                </li>
                <li>
                    <a href="[url]">[text]</a>
                </li>
            </ul>

            <a class="link-complex" href="[url]">
                <span class="link-complex__target">[text]</span>
                [text]
            </a>

            <input value="text" readonly>
        </article>
    </body>
</html>
```

Based on a work at [github.com/necolas/idiomatic-html](https://github.com/necolas/idiomatic-html). [Creative Commons Attribution 3.0 Unported Licensed](http://creativecommons.org/licenses/by/3.0/)
