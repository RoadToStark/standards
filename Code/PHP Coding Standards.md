PHP Coding Standards
-------------------


This document describes PHP coding standards for Phabricator and related
projects (like Arcanist and libphutil).

**Overview**

This document outlines technical and style guidelines which are followed in many projects. Contributors should follow these guidelines.

These guidelines are essentially identical to the Facebook guidelines. If you are already familiar with the Facebook guidelines, you can probably get away with skimming this document.

**Spaces, Linebreaks and Indentation**

  - Use 4 spaces for indentation. Don't use tab literal characters.
  - Use Unix linebreaks ("`\n`"), not MSDOS ("`\r\n`") or OS9 ("`\r`").
  - Use K&R style braces and spacing.
  - Put a space after control keywords like `if` and `for`.
  - Put a space after commas in argument lists.
  - Put a space around operators like `=`, `<`, etc.
  - Don't put spaces after function names.
  - Parentheses should hug their contents.
  - Generally, prefer to wrap code at 80 columns.

**Case and Capitalization**

  - Name variables and functions using `lowercase_with_underscores`.
  - Name classes using `UpperCamelCase`.
  - Name methods and properties using `lowerCamelCase`.
  - Use uppercase for common acronyms like ID and HTML.
  - Name constants using `UPPERCASE`.
  - Write `true`, `false` and `null` in lowercase.

**Comments**

  - Do not use `#` (shell-style) comments.
  - Prefer `//` comments inside function and method bodies.

**PHP Language Style**

  - Use `<?php`, not the `<?` short form. Omit the closing `?>` tag.
  - Prefer casts like `(string)` to casting functions like `strval()`.
  - Prefer type checks like `$v === null` to type functions like
    `is_null()`.
  - Avoid all crazy alternate forms of language constructs like `endwhile`
    and `<>`.
  - Always put braces around conditional and loop blocks.

**PHP Language Features**

  - Use PHP as a programming language, not a templating language.
  - Avoid `globals`.
  - Avoid `extract()`.
  - Avoid `eval()`.
  - Avoid variable variables.
  - Prefer classes over functions.
  - Prefer class constants over defines.
  - Avoid naked class properties; instead, define accessors.
  - Use exceptions for error conditions.
  - Use type hints, use `assert_instances_of()` for arrays holding objects.

**Examples**

- if/else:

```php
  if ($some_variable > 3) {
    // ...
  } else if ($some_variable === null) {
    // ...
  } else {
    // ...
  }
```
You should always put braces around the body of an if clause, even if it is only
one line long. Note spaces around operators and after control statements. Do not
use the "endif" construct, and write "else if" as two words.

- for:
```php
  for ($ii = 0; $ii < 10; $ii++) {
    // ...
  }
```
Prefer $ii, $jj, $kk, etc., as iterators, since they're easier to pick out
visually and react better to "Find Next..." in editors.

- foreach:
```php
  foreach ($map as $key => $value) {
    // ...
  }
```
- switch:
```php
  switch ($value) {
    case 1:
      // ...
      break;
    case 2:
      if ($flag) {
        // ...
        break;
      }
      break;
    default:
      // ...
      break;
  }
```
`break` statements should be indented to block level.

- array literals:
```php
  $junk = array(
    'nuts',
    'bolts',
    'refuse',
  );
```
Use a trailing comma and put the closing parenthesis on a separate line so that
diffs which add elements to the array affect only one line.

- operators:
```php
  $a + $b;                // Put spaces around operators.
  $arr[] = $element;      // Couple [] with the array when appending.
  $obj = new Thing();     // Always use parens.
```
- function/method calls:
```php
  // One line
  eject($cargo);

  // Multiline
  AbstractFireFactoryFactoryEngine::promulgateConflagrationInstance(
    $fuel,
    $ignition_source);
```
- function/method definitions:
```php
  function example_function($base_value, $additional_value) {
    return $base_value + $additional_value;
  }

  class C {
    public static function promulgateConflagrationInstance(
      IFuel $fuel,
      IgnitionSource $source) {
      // ...
    }
  }
```
- class:

```php
  class Dog extends Animal {

    const CIRCLES_REQUIRED_TO_LIE_DOWN = 3;

    private $favoriteFood = 'dirt';

    public function getFavoriteFood() {
      return $this->favoriteFood;
    }
  }
```
