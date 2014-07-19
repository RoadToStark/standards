Javascript Coding Standards
---------------------------

This document describes Javascript coding standards for Phabricator and Javelin.

**Overview** 

This document outlines technical and style guidelines which are followed in
many projects. Contributors should follow these guidelines. 

These guidelines are essentially identical to the Facebook guidelines. If you are already familiar with the Facebook
guidelines, you can probably get away with skimming this document.


**Spaces, Linebreaks and Indentation** 

  - Use two spaces for indentation. Don't use literal tab characters.
  - Use Unix linebreaks ("`\n`"), not MSDOS ("`\r\n`") or OS9 ("`\r`").
  - Use K&R style braces and spacing.
  - Put a space after control keywords like `if` and `for`.
  - Put a space after commas in argument lists.
  - Put space around operators like `=`, `<`, etc.
  - Don't put spaces after function names.
  - Parentheses should hug their contents.
  - Generally, prefer to wrap code at 80 columns.

**Case and Capitalization** 

The Javascript language unambiguously dictates casing/naming rules; follow those
rules.

 - Name variables using `lowercase_with_underscores`.
 - Name classes using `UpperCamelCase`.
 - Name methods and properties using `lowerCamelCase`.
 - Name global functions using `lowerCamelCase`. Avoid defining global
    functions.
 - Name constants using `UPPERCASE`.
 - Write `true`, `false`, and `null` in lowercase.
 - "Internal" methods and properties should be prefixed with an underscore.
    For more information about what "internal" means, see
    **Leading Underscores**, below.

**Comments** 

 - Strongly prefer `//` comments for making comments inside the bodies of
    functions and methods (this lets someone easily comment out a block of code
    while debugging later).

**Javascript Language** 

 - Use `[]` and `{}`, not `new Array` and `new Object`.
 - When creating an object literal, do not quote keys unless required.

**Examples** 

 - if/else

```javascript
  if (x > 3) {
    // ...
  } else if (x === null) {
    // ...
  } else {
    // ...
  }

```

- Ternary if/else

```js
var foo = (a === b)
  ? 1
  : 2;
```

You should always put braces around the body of an if clause, even if it is only
one line. Note that operators like `>` and `===` are also surrounded by
spaces.

- for (iteration)

```javascript
  for (var ii = 0; ii < 10; ii++) {
    // ...
  }
```

Prefer `ii`, `jj`, `kk`, etc., as iterators, since they're easier to pick out
visually and react better to "Find Next..." in editors.

- for (enumeration)

```javascript
  for (var k in obj) {
    // ...
  }
```

Make sure you use enumeration only on `Objects`, not on `Arrays`.

- switch

```javascript
  switch (x) {
    case 1:
      // ...
      break;
    case 2:
      if (flag) {
        break;
      }
      break;
    default:
      // ...
      break;
  }
```

`break` statements should be indented to block level. If you don't push them
in, you end up with an inconsistent rule for conditional `break` statements,
as in the `2` case.

If you insist on having a "fall through" case that does not end with `break`,
make it clear in a comment that you wrote this intentionally. For instance:

```javascript
  switch (x) {
    case 1:
      // ...
      // Fall through...
    case 2:
      //...
      break;
  }
```

**Leading Underscores** 

By convention, methods names which start with a leading underscore are
considered "internal", which (roughly) means "private". The critical difference
is that this is treated as a signal to Javascript processing scripts that a
symbol is safe to rename since it is not referenced outside the current file.

The upshot here is:

  - name internal methods which shouldn't be called outside of a file's scope
    with a leading underscore; and
  - **never** call an internal method from another file.

If you treat them as though they were "private", you won't run into problems.

Other little things...

**Use descriptive conditions**

Any non-trivial conditions should be assigned to a descriptively named variable or function:

*Right:*

```js
var isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);

if (isValidPassword) {
  console.log('winning');
}
```

*Wrong:*

```js
if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
  console.log('losing');
}
```

**Write small functions**

Keep your functions short. A good function fits on a slide that the people in
the last row of a big room can comfortably read. So don't count on them having
perfect vision and limit yourself to ~15 lines of code per function.

**Return early from functions**

To avoid deep nesting of if-statements, always return a function's value as early
as possible.

*Right:*

```js
function isPercentage(val) {
  if (val < 0) {
    return false;
  }

  if (val > 100) {
    return false;
  }

  return true;
}
```

*Wrong:*

```js
function isPercentage(val) {
  if (val >= 0) {
    if (val < 100) {
      return true;
    } else {
      return false;
    }
  } else {
    return false;
  }
}
```

Or for this particular example it may also be fine to shorten things even
further:

```js
function isPercentage(val) {
  var isInRange = (val >= 0 && val <= 100);
  return isInRange;
}
```

**Name your closures**

Feel free to give your closures a name. It shows that you care about them, and
will produce better stack traces, heap and cpu profiles.

*Right:*

```js
req.on('end', function onEnd() {
  console.log('winning');
});
```

*Wrong:*

```js
req.on('end', function() {
  console.log('losing');
});
```

**No nested closures**

Use closures, but don't nest them. Otherwise your code will become a mess.

*Right:*

```js
setTimeout(function() {
  client.connect(afterConnect);
}, 1000);

function afterConnect() {
  console.log('winning');
}
```

*Wrong:*

```js
setTimeout(function() {
  client.connect(function() {
    console.log('losing');
  });
}, 1000);
```

This coding standards are a mix of Facebook and Felix ones for Javascript & Node.js.
