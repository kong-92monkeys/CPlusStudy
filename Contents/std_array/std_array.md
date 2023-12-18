# std::array

## Abstract

- A thin wrapper around C-style arrays.
- Not automatically cast to a pointer.
- Have useful functionalities, such as iterators.

## Syntax

- Input the type and length of an array as template parameters.

```cpp
// Defined in <array>
std::array<int, 3> myArr1{ 1, 2, 3 };

// OK, thanks to CTAD
std::array myArr2{ 4, 5 };
```

## Usages

```cpp
std::array myArr{ 1, 2, 3, 0 };

// Get size
size_t arrSize{ myArr.size() };

// Get raw pointer
int *const data{ myArr.data() };

// Iteration
std::sort(myArr.begin(), myArr.end());
for (int &elem : myArr)
    ++elem;
```