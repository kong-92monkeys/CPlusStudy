# Attributes

## Abstract

- A mechanism to add optional information into source code.
    - It is similar to Java's annotations.
- Can be applied to various entities such as classes, functions, enums, etc.
- The compiler can alter the behavior based on the attributes.

## Syntax

- Enclose the attribute name within double square brackets.

```cpp
[[some_attribute_name]]
void myFunc()
{

}
```

## `[[fallthrough]]`

- When using the switch-case syntax, you can accidentally forget a `break` statement.
- Compilers might give a warning if the 'fallthrough' is detected.
- To suppress this warning, you can inform the compiler that it is intentional by using the `[[fallthrough]]` attribute.

```cpp
int myInt{ 2 };

switch (myInt) {
    case 0:
        [[fallthrough]];

    case 1:
        doSomething1();
        break;

    case 2:
        doSomething2();
        break;
}
```

## `[[nodiscard]]`

- Can be used on a function returning a value.
- it signals the compiler to issue a warning if the returned value is unused when the function is called.

```cpp
[[nodiscard]] int myFunc()
{
    return 1;
}

int main()
{
    // The compiler may issue a warning
    myFunc();
    return 0;
}
```

## `[[maybe_unused]]`

- Used to suppress the compiler from issuing a warning when something is unused.

```cpp
// Without the attribute,
// the compiler can issue warnings about unreferenced parameters.
int myFunc([[maybe_unused]] int param)
{
    return 1;
}
```

## `[[noreturn]]`

- Means that it never returns control to the call site.

```cpp
[[noreturn]] void forceTerminate()
{
    std::exit(1);
}
```

- Using the `[[noreturn]]` attribute informs the compiler about code lines that are guaranteed to be unreachable.

```cpp
[[noreturn]] void forceTerminate()
{
    std::exit(1);
}

int myFunc()
{
    if (checkError())
        return 1;

    forceTerminate();

    // No return value, but it's OK thanks to [[noreturn]].
    // return 0;
}
```

## `[[deprecated]]`

- Can be used to mark something as deprecated.
- If you use something deprecated, you¡¯ll get a warning.

```cpp
[[deprecated]] void oldFunc1()
{
    
}

[[deprecated("Use newFunc instead.")]] void oldFunc2()
{
    
}
```