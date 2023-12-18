# std::optional

## Abstract

- Holds a value of a specific type or nothing.
- This removes the need to return special values such as `nullptr`, `-1`, `false` and so on.

## Example

```cpp
// Defined in <optional>
std::optional<int> myInt;

// False
bool hasValue = myInt.has_value();

// Also false
hasValue = (myInt != std::nullopt);

// Cannot resolve value, std::bad_optional_access exception is thrown
// int value = myInt.value();
// int value = *myInt;
	
// Return the value or the default value if the value is empty.
int value = myInt.value_or(0);

// Emplace the value, copy or move
myInt.emplace(5);

// True
hasValue = myInt.has_value();
```