# GLSL Style Guide
<!-- TODO: Provide reasoning for these rules. -->

This guide is for the OpenGL Shading Language (GLSL).

## Table of Contents

- [Naming Convention](#naming-convention)
- [Comments and Documentation](#comments-and-documentation)
- [Indentation and Curly Braces](#indentation-and-curly-braces)
- [File Structure](#file-structure)

## Naming Convention

| Identifier          | Casing        | Naming Structure | Example                                          |
| ------------------- | ------------- | ---------------- | ------------------------------------------------ |
| Function    | camelCase     | Verb, verb-noun  | vec3 calcLighting(...) <br> float attenuation(...)               |
| Struct              | sPascalCase    | Noun             | struct sMaterial {...};                           |
| Macro, Constant     | ALL_CAPS      |                  | #define MACRO_NAME ... <br> const int BLACK = 3; |
| Parameter, Variable | camelCase     | Noun             | exampleText, numberLetters                       |
| Uniform | PascalCase | Noun | Material
| In/Out Variable | camelCase | Noun | texCoords, fragPos
| Attribute | aCamelCase | Noun | aPos, aNormal, aTexCoords

Refrain from using hungarian notation.

Built in variables such as gl_Position and FragColor do not follow this convention.

## Comments

Single-line comments should be formatted as in the following example:

```glsl
// This is a single-line comment.

// Header
```

Note the space separating the `//` and the comment. Comments should start with a capital letter, and sentences should end
with punctation. Headers/labels do not need to end with punctuation.

Multi-line comments should be formatted as in the following example:

```glsl
/*
 * A multi-line
 * comment.
*/
```

Note that the first and last lines are blank and the middle lines are indented with a space before the comments.

## Indentation and Curly Braces

All curly brackets follow Allman/BSD style - that is opening and closing braces are located on their own lines and are aligned with their control statement.
Code blocks should be indented within these braces.

Example:

```glsl
void main()
{
    // Stuff
}

struct sMaterial
{
    // Stuff
};
```

It is fine to omit braces for single-line statements, but code blocks should not appear on the same line as the control statement.
Use braces if it is possible the block may expand on in the future, so they do not have to be added later.

Example:

```glsl
// Okay
if (sandwich == tasty)
    eat(sandwich);

// Bad
if (sandwich == tasty) eat(sandwich);
```

## File Structure

Files should be structured in the following order:
- Shader profile
- Attributes, if any
- Output variables
- Input variables
- Uniforms
- Function prototypes
- Main function
- Function implementations

Example vertex shader:

```glsl
#version 430 core

layout (location = 0) in vec3 aPos;
layout (location = 1) in vec2 aTexCoords;

out vec2 texCoords;

uniform mat4 Model;
uniform mat4 ViewProjection;

void main()
{
    ...
}

```

Example fragment sahder:

```glsl
#version 430 core

out vec4 FragColor;

in vec2 texCoords;

uniform struct sMaterial
{
    sampler2D diffuse;
    sampler2D specular;
    float shininess;
} Material;

vec3 calcLighting(...);

void main()
{
    ...
}

vec3 calcLighting(...)
{
    ...
}
```
