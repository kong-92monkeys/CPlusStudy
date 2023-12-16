# Enumerations

## Abstract

- User-defined type that consists of a group of named integral constants called ***enumerators***.
- ***Scoped enumeration types*** was introduced in C++11.
- To modify the value scope of the enumerators, specify the desired integer type after the enum type name.
- From C++17, by defining an enum with an underlying type and no enumerators, you can in effect introduce a new integral type that has no implicit conversion to any other type.

## Legacy syntax from C

```cpp
enum EnumName
{
	ENUM1,
	ENUM2,
	ENUM3
}
```

- It calls ***unscoped enumeration type***.
- Not *type-safe*, and enumerators are just interpreted as integer values.

```cpp
enum Fruits
{
	APPLE,
	ORANGE,
	BANANA
};

// true
bool isApple = (APPLE == 0);

// true
bool isOrange = (ORANGE == 1);

// true
bool isBanana = (BANANA == (ORANGE + 1));
```

- The first enumerator is initialized to 0.
- Subsequent enumerators increment by 1 sequentially.
- You can also explicitly assign desired values.

```cpp
enum Fruits
{
	APPLE,			// 0
	ORANGE,			// 1
	BANANA = 10,	// 10
	LEMON			// 11
};
```

## *Scoped Enumeration Type* (Since C++11)

```cpp
enum class EnumName
{
	ENUM1,
	ENUM2,
	ENUM3
}
```

- Add the `class`(or `struct`) keyword after `enum`.
- Each enumerator becomes a named constant of the enum type.
- Need to prefix the type name to access the enumerators.
- No implicit conversions from enumerators to `int`.

```cpp
enum class Fruits
{
	APPLE = 3,
	ORANGE,
	BANANA
};

// Compile error
bool isApple = (Fruits::APPLE == 3);

// Compile error
bool isOrange = (Fruits::ORANGE == 4);

// OK
bool isBanana = (static_cast<int>(Fruits::BANANA) == 5);
```

## Enums with underlying type (Since C++11)

- By default, enumerators (scoped or not) have `int` values.
- If you want to modify the value scope of the enumerators, specify the desired integer type after the enum type name.

```cpp
enum class Fruits : uint64_t
{
	APPLE = 1000'2000'3000'4000ULL,
	ORANGE,
	BANANA
};
```

## Enums with no enumerators (Since C++17)

- With an explicit underlying type and no enumerators, you can in effect introduce a new integral type.
- No implicit conversion to any other type.

```cpp
enum class MyUInt : uint32_t {};

uint32_t stdUInt	{ 1U };
MyUInt myUInt		{ 2U };

// compile error
bool comp1 = (stdUInt == myUInt);

// OK
bool comp2 = (stdUInt != static_cast<uint32_t>(myUInt));
```

- This trick has also been used in implementing [std::byte](https://en.cppreference.com/w/cpp/types/byte).