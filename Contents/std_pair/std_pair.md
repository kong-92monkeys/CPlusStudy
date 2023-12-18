# std::pair

## Abstract

- Groups together two values of possibly different types.
- The values are accessible through the `first` and `second` public fields.

## Example

```cpp
// Defined in <utility>
std::pair<int, float> pair1 { 1, 2.0f };
int intVal                  { pair1.first };
float floatVal              { pair1.second };

// CTAD
std::pair pair2     { "myStr", 5U };
std::string strVal  { pair2.first };
uint32_t uintVal    { pair2.second };
```