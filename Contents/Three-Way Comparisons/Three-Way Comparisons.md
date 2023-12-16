# Three-Way Comparisons

## Abstract

- A comparison operator that was added in C++20.
- Resembling the shape of a spaceship (`<=>`).
- Indicates whether a value is equal to, less than, or greater than another value.

## Syntax

- It is a comparison operator.

```cpp
int lhs        { 0 };
int rhs        { 1 };
auto result    { lhs <=> rhs };
```

## `strong_ordering`

- If the operands are integers, the result is `strong_ordering` type.
- `strong_ordering` signifies that the operands are fully ordered and always comparable.

```cpp
// Defined in <compare>
std::strong_ordering result{ };
	
// true
result = (0 <=> 1);
bool isLess    { result == std::strong_ordering::less };

// true
result = (1 <=> 1);
bool isEqual    { result == std::strong_ordering::equal };

// true
result = (2 <=> 1);
bool isGreater  { result == std::strong_ordering::greater };
```

## `partial_ordering`

- If the operands are floating-point types, the result is `partial_ordering` type.
- `partial_ordering` signifies that the operands may sometimes be unorderable or incomparable.
    - In that case, `<, ==, >` operations can all be `false`.

```cpp
// Also defined in <compare>
std::partial_ordering result{ };
	
// defined in <limits>
float nan{ std::numeric_limits<float>::quiet_NaN() };

// true
result = (0.0f <=> 1.0f);
bool isLess        { result == std::partial_ordering::less };

// true
result = (1.0f <=> 1.0f);
bool isEquivalent  { result == std::partial_ordering::equivalent };

// true
result = (2.0f <=> 1.0f);
bool isGreater     { result == std::partial_ordering::greater };

// true
result = (nan <=> 1.0f);
bool isUnordered   { result == std::partial_ordering::unordered };
```

## `weak_ordering`

- There is also a `weak_ordering` similar to `partial_ordering`.
- While `weak_ordering` allows for being unordered, it must still be comparable.
    - One of a `<, ==, >` must be `true`.
- Not used in comparisons of built-in types but employed when overloading the operator in classes.