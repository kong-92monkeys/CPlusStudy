# Structured Bindings

## Abstract

- A way to declare multiple variables at once to receive a sequence of values from data structures like `array`, `struct`, `pair`, etc.

## Syntax

- Declare multiple variables consecutively within square brackets after the `auto` keyword.

```cpp
std::array myValues{ 1, 2, 3 };
auto [val1, val2, val3]{ myValues };
```

- The number of variables has to match the number of values in the expression on the right.
- Structured bindings also work with `struct`s if **all non-static members are** `public`.

```cpp
struct Size
{
    int width;
    int height;
};

Size size{};
auto [width, height]{ size };
```

- If you want to receive them in reference form, append the `&` symbol next to the `auto` keyword.
- Additionally, if you want to prevent modification of the values, add `const` before the `auto` keyword.

```cpp
Size size{};
auto &[width1, height1]{ size };
width1 = 10;
height1 = 20;

assert(size.width == 10);
assert(size.height == 20);

const auto [width2, height2]{ size };
// Error
// width2 = 10;
// height2 = 20;

// You can apply both, of course.
const auto &[width3, height3]{ size };
```