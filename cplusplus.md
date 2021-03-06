# C++ Style Guide

This guide is for the C++ programming language.

## Table of Contents

- [Naming Convention](#naming-convention)
- [Comments and Documentation](#comments-and-documentation)
- [Indentation and Curly Braces](#indentation-and-curly-braces)
- [Pointers](#pointers)
- [Constants](#constants)
- [Casting](#casting)
- [Sizeof](#sizeof)
- [Macros](#macros)
- [Functions](#functions)
  - [Reference Parameters](#reference-parameters)
- [Structures](#structures)
- [Classes](#classes)
  - [Data Members](#data-members)
  - [Member Order](#member-order)
- [Control Flow](#control-flow)
  - [Early Returns](#early-returs)
  - [Goto](#goto)
- [nullptr vs NULL](#nullptr-vs-null)
- [File Structure and Libraries](#file-structure-and-libraries)
  - [Includes](#includes)
  - [Include Guards](#include-guards)
  - [Using Namespace](#using-namespace)
  - [Boost](#boost)

## Naming Convention

| Identifier          | Casing        | Naming Structure | Example                                          |
| ------------------- | ------------- | ---------------- | ------------------------------------------------ |
| Class               | PascalCase    | Noun             | class Example {...};                             |
| Enum, Enum member   | PascalCase    | Noun             | enum Color { Red, Green, Blue };                 |
| Function, Method    | camelCase     | Verb, verb-noun  | void print() <br> void printItem()               |
| Interface           | IPascalCase   | Noun             | class IDictionary {...};                         |
| Struct              | PascalCase    | Noun             | struct Vector2D {...};                           |
| Macro, Constant     | ALL_CAPS      |                  | #define MACRO_NAME ... <br> const int BLACK = 3; |
| Parameter, Variable | camelCase     | Noun             | exampleText, numberLetters                       |
| Template parameter  | TPascalCasing | Noun             | T, TItem, TPolicy                                |

Refrain from using hungarian notation. The exception is when naming UI controls. Be consistent when using prefixes and suffixes.

## Comments and Documentation

Single-line comments should be formatted as in the following example:

```cpp
// This is a single-line comment.

// Header
```

Note the space separating the `//` and the comment. Comments should start with a capital letter, and sentences should end
with punctation. Headers/labels do not need to end with punctuation.

Multi-line comments should be formatted as in the following example:

```cpp
/*
 * A multi-line
 * comment.
*/
```

Note that the first and last lines are blank and the middle lines are indented with a space before the comments.

Documentation should be located in the *header* file above the method declaration, using Doxygen comments as in the following example:

```cpp
/**
 * @brief  Example public method.
 * @note   Documented with Doxygen.
 * @param  a: example variable
 * @param  b: example variable
 * @retval Example as a double.
 */
double ExampleClass::ExampleMethod(int a, bool b);
```

See [doxygen.org] for information on using Doxygen.

## Indentation and Curly Braces

All curly brackets follow Allman/BSD style - that is opening and closing braces are located on their own lines and are aligned with their control statement.
Code blocks should be indented within these braces.

Example:

```cpp
int main()
{
    // Stuff
}

class Car
{
    // Stuff
};
```

Exception: When braces are placed after an assignment operator (=), the opening brace should be located on the same line.

```cpp
int array[] = {
    1, 0, 0,
    0, 1, 0,
    0, 0, 1
};
```

It is fine to omit braces for single-line statements, but code blocks should not appear on the same line as the control statement.
Use braces if it is possible the block may expand on in the future, so they do not have to be added later.

Example:

```cpp
// Okay
if (sandwich == tasty)
    eat(sandwich);

// Bad
if (sandwich == tasty) eat(sandwich);
```

## Pointers

All pointers should be initialized when they are declared. They should be reinitialized to `nullptr` after being freed.

Example:

```cpp
int *p = nullptr;
Window *window = new Window();

delete p;
p = nullptr;
delete window;
window = nullptr;
```

Dereference and address-of operators (`*` and `&`) should be right-aligned with the pointer name.

Example:

```cpp
int *a = 1;
int *b = &a;
```

## Constants

Prefer using const over `#define`. Only use `#define` when preprocessing is actually necessary.

Example:

```cpp
// Good
const int BLACK = 1;

// Bad
#define BLACK 1
```

## Casting

Use C++ casts instead of C-style casts. Examples are static_cast, reinterpret_cast, const_cast, and dynamic_cast.

Example:

```cpp
// Good
double d = 23.5;
int i = static_cast<int>(d);

// Bad
int i = (int)d;
```

## Sizeof

Use `sizeof(var)` instead of `sizeof(Type)`. Do not write the known size or type of a variable. Reference the variable name instead.
The exception is when there is not a related variable to reference; then it is okay to use `sizeof(Type)`;

Example:

```cpp
// Good
float f = 2.0f;
sizeof(f);

// Bad
sizeof(float);
```

## Macros

Only use macros when absolutely necessary. Platform-dependant code is an example of acceptable use.

Example:

```cpp
// Good
if (a == b)
    doSomething();

// Bad
#if a == b
    doSomething();
#endif
```

## Functions

### Reference Parameters

Refrain from using reference parameters as output because it makes it difficult to read what the output of a function is.

Example:

```cpp
// Good
int sum(int a, int b)
{
    int x = a + b;
    return x;
}

// Bad
void sum(int a, int b, int &sum)
{
    sum = a + b;
}
```

## Structures

Use structures instead of classes to define a collection of data that does not contain functions. Only use classes if the data structure includes functions.
This is to make intention clearer, even though structs in C++ do support member functions.

Example of when to use a structure:

```cpp
struct Vector2
{
    float x;
    float y;
};
```

Example of when to use a class:

```cpp
class Vector2
{
public:
    float x;
    float y;

    float length();
};
```

## Classes

### Data Members

Do not declare public data members; use inline accessors instead.

Initialize member variables in the same order they are defined in the class declaration.

Prefer initialization instead of assignment in constructors.

Example:

```cpp
// Good
Example(int width, int height) : width(width), height(height) { }

// Bad
Example(int width, int height)
{
    this->width = width;
    this->height = height;
}
```

### Member Order

Access modifiers should be grouped in the following order:

- public
- protected
- private

Members within the above groups should be grouped in the following order:

- Enums
- Static fields
- Constants
- Fields
- Constructors
- Destructors
- Accessors
  - Getter
  - Setter
- Interface implementations
- Static member functions
- Non-static member functions
- Virtual member functions
- Nested types

## Control Flow

### Early Returns

Do return early except when having a single return point improves performance or readability.

### Goto

Do not use goto statements.

## nullptr vs NULL

Use `nullptr` instead of `NULL`. Also do not use `0` in their place, nor the opposite.

## File Structure and Libraries

Header and source files should be named in camelCase.

Source files should use the `*.cpp` file extension and not the less common `*.cxx` or `*.cc` extensions.

Header files should use the `*.hpp` file extension because it makes it clear that the header is written for C++.
The exception is when writing headers that are meant to also support C, in which case use the `*.h` extension.

### Includes

Includes should be placed at the top of a file (below any copyright/file header comments) in the following order:

- Project headers using `#include "example.hpp"`
- Libraries local to the project using `#include "library/example.hpp"`
- Libraries elsewhere in the system using `#include <library/example.hpp>`
- System headers using `#include <example.hpp>`
- Standard headers using `#include <example>`

Example:

```cpp
#include "log.hpp"
#include "freetype/freetype.h"
#include <GLFW/glfw3.h>
#include <windows.h>
#include <string>
```

Use C++ headers instead of C headers, for example use ctime instead of time.h.

Include headers in *all* files they are used in. For example, if a file is used in both a class's header and source files, include it in both. This ensures files are not dependant on other files' includes.

### Include Guards

Use include guards in the following format for all header files:

```cpp
#ifndef HEADER_NAME_HPP
#define HEADER_NAME_HPP
// header code
#endif
```

Do not use `#pragma once`. Most modern compilers support it somehow, but it is still not standard.

Include the project name, subdirectory (if any), file name, and extension in the `#define`. For a file called "log.hpp", in a "debug" subfolder, belonging to a project called "Game", use `GAME_DEBUG_LOG_HPP`. This prevents potential conflicts between two projects/libraries that have similar header names, while being more readable than a GUID.

### Using Namespace

Do not use `using namespace std;` anywhere. This can cause issues when using libraries with conflicted type names. Prefer prefixing with `std::`.

### Boost

Prefer standard C++ types instead of Boost types, for example use the built-in `<memory>` and `<filesystem>` for smart pointers and file system operations. 
